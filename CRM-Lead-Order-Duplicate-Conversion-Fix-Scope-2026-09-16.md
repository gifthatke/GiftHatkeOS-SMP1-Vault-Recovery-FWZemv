# CRM Lead-to-Order Duplicate-Conversion — Fix Scope

**Date:** 2026-09-16
**Status:** SCOPE DOCUMENT ONLY — NOT AN IMPLEMENTATION AUTHORIZATION
**Origin:** GAP-001's Domain 19 second pass (`GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md`, lines 1137-1233), registered as consolidated plan Tier 2 item 7
**Governs:** the non-atomic order-creation / lead-conversion-marking write pair in `orderCreateFromLeadService.createOrderFromLead`

This document is a precise implementation scope, built from a direct, complete read of the affected Standalone code (`apps/api/src/order-create-from-lead-service.ts`, `apps/api/src/order-production-mutation-coordinator.ts`, `apps/api/src/server.ts`'s wiring, and `packages/platform/src/order.ts`'s persistence adapter), plus the original GAP-001 finding. It is not a patch and contains no code. Writing or merging anything described here requires its own separate, explicit authorization. This document was requested in place of implementing the fix directly, specifically because the fix touches the transaction boundary of a working, already-tested subsystem (Order creation) rather than adding new, isolated capability — the same caution this session has applied consistently to load-bearing shared code.

## 1. What's wrong

`orderCreateFromLeadService.createOrderFromLead` (`apps/api/src/order-create-from-lead-service.ts:479-588`) does two separate, independently-committed writes:

1. `await options.createOrder(input)` — persists the Order and its items.
2. `await options.markLeadConverted(lead.id, orderId, actor)` — marks the source CRM lead as converted, pointing at the new order.

A code comment directly above the second call names this ordering as deliberate frozen parity:

```
/*
 * Frozen ordering:
 * 1. Order creation/persistence succeeds.
 * 2. CRM is then marked converted.
 *
 * Never mark the lead before Order creation succeeds.
 */
```

If step 2 fails after step 1 has already committed — a crash, a dropped connection, a timeout — the order exists, but the lead's `convertedOrderId` guard (checked at the top of the same function, `:506-516`) is never set. A retry of the same conversion request re-reads the still-unconverted lead, passes the guard, and creates a **second, duplicate order** from the same lead. The failure window is narrow (an actual write failure at exactly the wrong moment), but the consequence — a real duplicate order reaching Production and Shipping — is directly customer-visible, which is why the consolidated plan ranks it above the otherwise-similar item 8 (Inventory BOM consumption, now fixed) for business impact.

**This is not unique to CRM lead-conversion.** GAP-001's Domain 19 second pass (lines 1221-1233) found the identical failure shape recurring in **frozen's own** `ProcurementService.js` PR-to-PO conversion: create the PO and its items, then mark the source PR converted as a separate final write — if that final write fails, the PO exists but the PR's guard never sets, and a retry creates a duplicate PO. Per TITAN LOCK, no correction to frozen was proposed there; the passage instead noted that Standalone's own Procurement module — unlike frozen's — had already solved this shape of problem correctly elsewhere, and flagged that pattern as available to close the CRM finding, "if a correction to that specific finding is ever authorized." This document is that authorization request.

## 2. Confirmed: Standalone already has two working examples of the correct pattern — one of them directly in the Order module itself

### 2.1 Procurement's `createPurchaseOrder` (`apps/api/src/procurement-po-service.ts:571-963`)

Re-verified by direct read for this document (not assumed from the consolidated plan's summary). `createPurchaseOrder` opens one `options.transactions.runInTransaction(async (scope) => {...})` (`:604-608`), and inside that single callback: validates the source PR's conversion eligibility, builds the PO and its items, and — only if a source PR was supplied — builds the converted-PR record. Both `scope.purchaseOrders.savePurchaseOrder(purchaseOrder)` and `scope.purchaseRequisitions.savePurchaseRequisition(convertedPr)` (`:948-960`) are called from inside that same callback, with an explicit comment confirming the intent:

> "The existing Platform coordinator supplies both repositories from one PostgreSQL transaction. PO persistence and optional PR conversion therefore commit or roll back together."

This is genuinely correct, tested, shipped code — Standalone's Procurement module does not reproduce frozen's own bug here, unlike CRM lead-conversion, which does.

### 2.2 Order's own `updateWithoutActivity` — a closer, more directly analogous precedent, already solving this exact shape of cross-domain-write problem inside the Order module (`packages/platform/src/order.ts:2427-2517`, wired via `apps/api/src/server.ts:1218-1284`)

This is the more relevant precedent, and was not previously connected to item 7 in the consolidated plan's own text. `updateWithoutActivity` takes an **optional second parameter**, `resolveCustomerId?: OrderUpdateCustomerResolver`, typed as:

```ts
export type OrderUpdateCustomerResolver = (
  runtime: DatabaseRuntime,
) => Promise<string>;
```

Inside `updateWithoutActivity`'s own `runInTransaction(runtime.db, async (trx) => {...})`, after the order's optimistic-revision check succeeds, it calls `resolveCustomerId({db: trx, async destroy() {}})` (`:2476-2488`) — wrapping the transaction handle `trx` in a `DatabaseRuntime`-shaped object so the caller can construct **any** ordinary platform adapter against it, unchanged from how that adapter is normally constructed outside a transaction. `server.ts`'s wiring (`:1234-1283`) supplies exactly this: a resolver that constructs `createPlatformCustomerOrderAdapter` bound to the transaction runtime and calls `resolveOrUpsertCustomer` on it — so the Customer-record write commits or rolls back atomically with the Order update. A code comment there names this as its own prior GAP-001 fix, for the identical failure shape this document is about, just for Customer-upsert-on-update instead of CRM-lead-conversion-on-create:

> "do not resolve/upsert the customer here... pass a resolver that `baseOrderMutationPersistence.updateWithoutActivity` invokes itself, inside its own transaction... using a customer adapter scoped to that same transaction so the customer write is atomic with the order write."

**The asymmetry this document closes**: `createWithoutActivity` (`packages/platform/src/order.ts:2284-2425`) has no equivalent resolver parameter — it only ever persists the order and its items inside its own `runInTransaction`, with an explicit comment marking the boundary: "Intentionally stop after Order + Items persistence. Activity is owned by the later coordinator phase." That boundary is correct and should not move — this document does not propose touching it. What's missing is a way to inject *one more* transaction-scoped write, the same way `updateWithoutActivity` already can, for the one case (lead-conversion-on-create) that currently has to reach for it and can't, so it falls back to a second, separately-committed call instead.

## 3. Proposed scope

Extend `createWithoutActivity` with the same shape `updateWithoutActivity` already has, and wire `orderCreateFromLeadService` to use it instead of its current two-call sequence. Four files, in dependency order:

**3.1 `packages/platform/src/order.ts`**
- Add a type mirroring `OrderUpdateCustomerResolver`, e.g. `OrderCreateSideEffectResolver = (runtime: DatabaseRuntime, created: {order: Order; items: readonly OrderItem[]}) => Promise<void>` (return type `void`, not `string` — this resolver doesn't feed a value back into the order being written, unlike the customer-ID resolver; it just needs to run inside the same transaction).
- Add an optional second parameter to `PlatformOrderMutationPersistenceAdapter.createWithoutActivity` and its implementation, invoked inside the existing `runInTransaction(runtime.db, async (trx) => {...})` callback, after the order+items inserts succeed and before the callback returns — mirroring exactly where `resolveCustomerId` is invoked in `updateWithoutActivity`, just after persistence instead of before/during it (there is no revision check to gate on here, since this is a create, not an update).

**3.2 `apps/api/src/order-production-mutation-coordinator.ts`**
- `OrderMutationPersistencePort.createWithoutActivity` needs the same optional second parameter added to its type, so the coordinator can pass it through.
- `OrderProductionMutationCoordinator.createOrder` needs a way to accept and forward this resolver from its own caller. The cleanest option (not decided here — see §5): add it as a third argument to `createOrder(input, sideEffect?)`, rather than folding it into `CreateOrderApplicationInput`, since it's a transaction-plumbing concern specific to this one caller (`orderCreateFromLeadService`), not a general property of "create an order."

**3.3 `apps/api/src/server.ts`**
- `customerAwareOrderMutationPersistence.createWithoutActivity` (`:1174-1216`) needs to forward the new resolver parameter through to `baseOrderMutationPersistence.createWithoutActivity`, the same way `updateWithoutActivity` already forwards `resolveCustomerId` today.
- `orderCreateFromLeadService`'s construction (`:1321-1354`) needs its `markLeadConverted` callback removed from `OrderCreateFromLeadServiceOptions` and replaced by passing a side-effect resolver into `createOrder` instead, built the same way the existing `resolveCustomerId` resolver is: `createCrmOrderAdapter(transactionRuntime, {clock: platformRuntime.crmClock, identities: platformRuntime.crmIdentities, actor: () => actor})`, then calling `.markLeadConverted(leadId, order.id)` on it — all already-existing, already-tested constructor functions (`createCrmOrderAdapter` already takes a `DatabaseRuntime` as its first argument today, confirmed by direct read, `packages/platform/src/crm.ts:770-788`), no new platform primitives required.

**3.4 `apps/api/src/order-create-from-lead-service.ts`**
- Remove `markLeadConverted` from `OrderCreateFromLeadServiceOptions`.
- Replace the current two-call sequence (`:547-579`) with one call to `options.createOrder(input, sideEffectResolver)`, where `sideEffectResolver` is constructed from `options` (a new dependency replacing `markLeadConverted`, shaped as something that can build a transaction-scoped CRM adapter — the exact shape is a design choice, not decided here, see §5).
- The `alreadyConverted` short-circuit at the top of `createOrderFromLead` (`:506-516`, checking `lead.convertedOrderId` before doing any work) is unaffected and stays exactly as-is — this document only changes what happens *after* that check has already passed.

## 4. What this deliberately does not touch

- **`orderProductionMutationCoordinator.createOrder`'s own three-step sequence** (persist → `production.synchronizeOrder` → `activities.addActivity`, `order-production-mutation-coordinator.ts:101-140`) stays exactly as it is today: only the first step (persistence) gains the new resolver hook; production-sync and activity-logging remain separate, non-transactional follow-up steps, matching their current status quo everywhere else in the codebase (every other `createOrder` caller, not just lead-conversion, already accepts this). Pulling those into the same transaction is a materially larger, unrelated change with its own tradeoffs (a Production-sync failure would then also roll back a successfully-created order) and is out of scope for closing the specific defect named in item 7.
- **`updateWithoutActivity`'s existing `resolveCustomerId` mechanism** is untouched — this document adds a sibling capability to `createWithoutActivity`, not a change to the update path.
- **Frozen** — per TITAN LOCK, no correction to frozen's `orderCreateFromLead` or `ProcurementService.js` is proposed; both keep their non-atomic ordering as the permanent reference baseline. Only Standalone's own reimplementation changes.
- **Item 9** (Shipping-to-Order status sync's silent-failure question) — a different, still-open finding requiring a business-intent decision before it can even be scoped, unrelated to this one beyond sharing a general "cross-module write consistency" theme.

## 5. Open design question, not decided here

How the CRM-marking side effect gets from `server.ts`'s wiring into the resolver `createOrderFromLeadService` passes to `createOrder` is a small design choice with at least two reasonable shapes:

- **(a)** `OrderCreateFromLeadServiceOptions` gains a `markLeadConvertedResolver: (leadId: string, orderId: string, actor: string) => OrderCreateSideEffectResolver` factory — `createOrderFromLead` calls it once it has `orderId`, and passes the result to `createOrder`. Keeps `order-create-from-lead-service.ts` free of any direct platform import.
- **(b)** `OrderCreateFromLeadServiceOptions` gains the resolver-factory ready-made per call, with `server.ts` doing the `leadId`/`actor` closure itself, similar to how `markLeadConverted` is wired today (`:1332-1353`) — `createOrderFromLead` would then only need to pass it straight through to `createOrder`, needing no change to its own signature-construction logic.

Either is small and low-risk; this document flags the choice rather than making it, consistent with how `§4.3` of the Executive Dashboard scope document left its own design questions open for implementation time.

## 6. Verification plan for implementation time

- A new test proving the atomicity itself: construct a `createOrder` resolver that throws after order+items would otherwise have committed, and assert that **no order row exists afterward** (the transaction rolled back) — mirroring the style already used for item 8's "shortage preflight blocks all posting" test (`apps/api/test/production-inventory-consumption-service.test.mjs`).
- A test confirming the lead stays unconverted (so a retry is safe) when the resolver throws.
- A test confirming the existing "already converted" short-circuit still returns immediately without calling `createOrder` at all (unchanged behavior, `order-create-from-lead-service.ts:506-516`).
- Existing `order-create-from-lead-service`/`order-production-mutation-coordinator`/`server.ts` wiring tests should be re-run and updated for the new signatures, the same way item 8's existing test suite was rewritten rather than left mocking the old shape.
- Full monorepo build and the full domain/platform/api regression suite, per this session's standing verification practice.

## 7. Suggested sequencing, not an authorization

If authorized: (1) the platform-layer signature change (§3.1) first, since every other file depends on it and it's the smallest, most mechanical piece — a direct structural mirror of `updateWithoutActivity`'s already-shipped `resolveCustomerId`; (2) the coordinator/server.ts plumbing (§3.2-§3.3); (3) `order-create-from-lead-service.ts`'s own rewiring (§3.4), resolving §5's open design question at that point; (4) tests (§6). Nothing here blocks on item 9 or any other open item.

## 8. Implementation record, 2026-09-16

Authorized and implemented in this session, following §7's own sequencing exactly. **Nothing has been committed or pushed.**

**§5's open design question, resolved**: option (a) — `OrderCreateFromLeadServiceOptions` gained a `markLeadConvertedResolver(leadId, actor): OrderCreateSideEffect` factory, keeping `order-create-from-lead-service.ts` free of any platform/database import, matching its existing style exactly. `createOrderFromLead` now calls `options.createOrder(input, options.markLeadConvertedResolver(lead.id, actor))` in place of the old two-call sequence — the old `options.markLeadConverted` dependency is gone entirely, not left dead.

**A real architectural constraint found and worked around, not assumed away**: while wiring `server.ts`, a doc comment I wrote for `OrderCreateSideEffect` (in `order-production-mutation-coordinator.ts`) that merely *mentioned* the forbidden token strings by name (`DatabaseRuntime`, `@gifthatkeos/platform`, `@gifthatkeos/database`, inside prose explaining the constraint) broke that file's own source-regex boundary test — the test does a naive substring search over the whole file, comments included, not just code. Reworded to describe the constraint without naming the tokens literally. Separately, and more substantively: `server.ts` turned out to have its own test (`runtime-composition-boundary.test.mjs`, "concrete Platform dependency is confined to the production server bootstrap") explicitly forbidding `@gifthatkeos/database` from ever being imported there, even as a type-only import — not just the neutral-coordinator files §2.2 already knew about. The original plan's `runtime as DatabaseRuntime` cast, which would have needed exactly that import, was therefore not viable as scoped. Fixed by narrowing `runtime` structurally instead: `runtime as Parameters<typeof createCrmOrderAdapter>[0]` — deriving the exact expected type off the already-imported `createCrmOrderAdapter` function itself, rather than naming the concrete runtime type at all. Fully type-safe, and needs no import `server.ts` doesn't already have.

**Built**, exactly matching §3's file-by-file plan:
- `packages/platform/src/order.ts`: added `OrderCreateSideEffectResolver` (mirroring `OrderUpdateCustomerResolver`'s shape); `createWithoutActivity` gained the optional `onCreated` parameter, invoked inside its existing `runInTransaction` callback after the Order + Items inserts, before the return — the literal `return { order, items };` shape itself was kept unchanged (not refactored into an intermediate variable), since a platform-test asserts that exact pattern by regex and there was no need to disturb it.
- `apps/api/src/order-production-mutation-coordinator.ts`: added the opaque `OrderCreateSideEffect` type (§3.2 exactly as scoped) and threaded it through `createOrder`.
- `apps/api/src/server.ts`: `customerAwareOrderMutationPersistence.createWithoutActivity` forwards the resolver through to the base platform adapter (unchanged from plan); `orderCreateFromLeadService`'s construction replaced `markLeadConverted` with `markLeadConvertedResolver`, building a CRM adapter bound to the transaction-scoped runtime it receives, per §3.3 (worked around the casting constraint as described above).
- `apps/api/src/order-create-from-lead-service.ts`: rewired per §3.4 and §5's resolved design question.

**What this did not touch**, confirmed unchanged after implementation: `orderProductionMutationCoordinator.createOrder`'s own three-step sequence (persist → `production.synchronizeOrder` → `activities.addActivity`) still runs exactly as before — only the persistence step gained the new hook. No other caller of `createOrder` (the general order-creation HTTP route) needed any change, since the new parameter is optional and TypeScript allows a narrower-arity caller to satisfy a wider-arity function type. No new permission key or route was needed — this is a pure internal-composition change to an existing write path.

**Verification**: extended `packages/platform/test/order-production-mutation-adapter.test.mjs` (2 new regex-structure tests, matching that file's existing convention — no live-database testing exists for this adapter today, consistent with how item 8's own transaction fix was verified at the unit/mock level, not against real Postgres) and `apps/api/test/order-create-from-lead-service.test.mjs` (rewrote the shared fixture to simulate `createOrder` invoking the resolver the way the real transaction does, added 2 new tests: one confirming the resolver is built from the fetched lead + caller's actor and reaches `createOrder` as its second argument with the right values, one confirming a failure inside the resolver — standing in for a rolled-back transaction — fails the whole `createOrderFromLead` call rather than reporting a false success). All existing tests in both files, plus the two boundary tests the fix initially broke (fixed as described above), pass unmodified in intent. Full monorepo build clean. Full regression suite re-run: domain 339/339 (unchanged), platform 233/233, api 666/667 — the one failure is the same pre-existing, unrelated `reports-routes.test.mjs` timezone issue tracked since PHB-1.

**This closes consolidated plan Tier 2 item 7.** The same failure shape recurring in frozen's own `ProcurementService.js` PR-to-PO conversion (§1) is not touched — per TITAN LOCK, frozen stays as the permanent reference baseline.
