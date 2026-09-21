# GAP-001 — Consolidated Pre-Handover Blockers and Ordered Implementation Plan

**Date:** 2026-09-15
**Status:** PLANNING DOCUMENT ONLY — NOT AN IMPLEMENTATION OR DEPLOYMENT AUTHORIZATION
**Origin:** consolidates every blocker found across this continuation session's GAP-001 domain-by-domain audit (`GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md`)
**Context:** Standalone has not yet been handed over to employees. Every item below is a pre-handover implementation gap, not a live-incident report.

This document does not authorize writing or merging any code. Each item below requires its own separate authorization, scope-lock, implementation, tests, regression, and closure — the same convention every other module in this repository has followed. Where a detailed scope document already exists for an item, it is linked rather than repeated.

## How to read this

Items are grouped by whether they block employees using Standalone for real work, not by how interesting the finding was. Within each tier, items are ordered by a rough blocking-severity/dependency judgment, not a strict priority ranking — several items in the same tier could reasonably be reordered by whoever authorizes the actual work.

---

## Tier 1 — Blocks core operations; should be resolved before handover

### 1. PHB-1 — Finance Transaction Write Workflows — ✅ IMPLEMENTED, 2026-09-15 (not yet committed)

**Status update:** authorized and implemented in this session. Four of the five identified actions are built, tested, and typecheck-clean in the working tree: Record Receipt (`POST /finance/receipts`), Record Expense (`POST /finance/expenses`), Pay Expense (`POST /finance/expenses/:expenseId/pay`), and Reverse Transaction (`POST /finance/transactions/:transactionId/reverse`). The fifth, Record Income, was explicitly decided against by the operator after direct re-verification found it has no permission check anywhere in frozen and its only real caller is a disposable self-test script — not a confirmed employee workflow; see `Finance-Write-Workflows-Pre-Handover-Scope-2026-09-15.md` §8 for the full record of that decision. **Nothing has been committed or pushed** — this exists in the working tree pending separate explicit authorization.

**What was missing (original framing, for record):** No way to record a customer receipt, record an expense, settle a pending expense, or reverse a transaction anywhere in Standalone's code. Frozen has real, working, permission-checked functions for all four (`financeRecordReceipt`, `financeRecordExpense`, `financePayExpense`, `financeReverseTransaction`). Standalone's Finance domain types and repository ports already existed, but the orchestration layer and HTTP routes did not — and the existing foundation itself had a real defect: 7 Finance tables' repository configs pointed at the wrong database column (e.g. `finance_product_costs` was configured for `product_cost_id`, the real column is `cost_id`). The `finance_period_locks` mismatch was fixed as part of this implementation, since every money action's posting-guard check depends on it; the other six remain unfixed (out of PHB-1's core scope).

**A second defect found and fixed during implementation, not in the original scope:** the generic Finance repository's `update()` is a bare upsert with no existence or state check — two concurrent "settle this expense" or "reverse this transaction" requests could both have succeeded. Fixed with narrow, conditionally-guarded writes for both actions rather than threading a revision field through the whole Finance persistence layer. Full detail in the scope document §8.

**Why Tier 1:** without this, employees handed Standalone cannot record that a customer paid, or that money went out, through the software at all. This is the most direct blocker to actual day-one use of Finance. Now resolved for the four actions that matter for that purpose.

**Full scope:** `Finance-Write-Workflows-Pre-Handover-Scope-2026-09-15.md`, §2, §7 (independent-review corrections), and §8 (implementation record).

**Correction, 2026-09-21:** "✅ IMPLEMENTED" above described the backend only. No frontend called any of these four routes until 2026-09-21 — Employee Web's Finance workspace was a read-only summary view with zero forms. See the 2026-09-21 status update below for the frontend closure and a real persistence defect it surfaced.

### 2. PHB-3 — Order Attachments — ✅ IMPLEMENTED, 2026-09-15 (not yet committed)

**Status update:** authorized and implemented in this session. `GET`/`POST /orders/:orderId/attachments` are built, tested, and typecheck-clean in the working tree — attachment row and activity-log entry written in one shared transaction, https:// enforced at both the application layer and the database (a check constraint), and a new `orders.attachments.create`/`.read` permission pair. One necessary deviation: frozen's attachment-ID prefix (`"OA-"`) collides with an ID space Standalone's own `order_activities` table already uses, so attachments use `"ATT-"` instead — same generation method, different prefix, forced by a pre-existing Standalone naming choice, not a free design decision. Full record in `Order-Attachments-Pre-Handover-Scope-2026-09-15.md` §7. **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** Canon explicitly names Attachments as a permanent Order Domain capability (§10.4). Frozen implements it fully (`OrderWorkspace.js`: `Order_Attachments`, HTTPS-URL validation, category/name/source/description, activity logging). Standalone had no attachment collection, route, or persistence of any kind before this implementation.

**Why Tier 1, not Tier 2:** this is not an edge case or a rare failure mode — it's routine, everyday functionality (attaching a reference file, a customer-supplied image, a signed document to an order) that frozen users have and Standalone users would immediately notice missing on day one.

**Full scope:** `Order-Attachments-Pre-Handover-Scope-2026-09-15.md`, §1–§6 (original scope) and §7 (implementation record).

### 3. PHB-5 — Today's Work / Task & Work Board — ✅ IMPLEMENTED, 2026-09-15 (not yet committed)

**Status update:** authorized and implemented in this session, both halves. Task store (§4.1): `POST /work-tasks`, `/assign`, `/block`, `/unblock`, `/complete`, `DELETE /work-tasks/:taskId` (archive), and `GET /work-tasks`, backed by a new `work_tasks` table and a new `TodaysWork` permission module (`workspace: ["read"]`, `tasks: ALL_ACTIONS`). Aggregator (§4.2): `GET /work-tasks/today`, composing the task store with five already-existing read methods across CRM/Orders/Production/Shipping/Customer Approval — each collector independently try/caught, so one unavailable source degrades to a warning rather than breaking the whole feed, matching frozen exactly. The priority-score/due-state logic (`workPriorityScore_`/`workDueState_`) and the aggregator's own sort/metrics logic were ported close to verbatim, including exact score weights and sort order. §4.2's flagged risk — field-level status-vocabulary translation, since Standalone's field names differ from frozen's — was resolved by a dedicated research pass against each source service's actual current code before writing the aggregator, not assumed; full translation table in `Task-Work-Board-Pre-Handover-Scope-2026-09-15.md` §9, including one genuine structural gap found (Customer Approval has no cross-order listing method in Standalone, resolved as a bounded N+1 over Orders already filtered to pending-approval status, not a new port method on a separately-owned module). Full record in that document's §8 (task store) and §9 (aggregator). **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** frozen's `TaskService.js` + `WorkService.js` implement a real, tested, UI-wired task-tracking board — employees create, assign, block, and complete tasks tied to any module (Orders, CRM, Production, Shipping, Approvals), aggregated into a priority-scored "Today's Work" feed that is a live, reachable part of frozen's UI (`Script.html`'s `.tw-assign`/`.tw-complete`/`.tw-block`/"New Task" bindings, backing `View_TodaysWork.html`). A direct, re-verified repo-wide search of Standalone found zero matches for any task-board concept anywhere in `packages/domain/src`, `packages/platform/src`, or `apps/api/src` before this implementation.

**Why Tier 1:** same basis as Attachments (item 2) — this is routine, everyday functionality employees actually use to coordinate daily work across modules, not an edge case or a rare failure mode. Stronger than initially recorded: frozen's `Router.js` sets `DEFAULT_MODULE: "TodaysWork"` — this is literally the first screen every employee sees on login, ahead of even the Dashboard (PHB-6). Now resolved: both the task store and the cross-module daily-queue view exist in the working tree, matching what frozen's own default-login screen actually shows.

**Full scope:** `Task-Work-Board-Pre-Handover-Scope-2026-09-15.md`, §1–§7 (original scope), §8 (task-store implementation record), and §9 (aggregator implementation record).

### 4. PHB-6 — Executive Dashboard first-screen content — 🟢 FULLY IMPLEMENTED, 2026-09-16 (not yet committed)

**Status update:** authorized and implemented across two sessions, both phases. Phase A (required fields, 7 KPI cards, system health): the Dashboard's existing `GET /dashboard/workspace` route returns an additive `executive` field with the greeting, KPI cards (Today's Sales, Monthly Revenue, Orders Today, Average Order Value, Gross Profit, Net Profit, Cash Position), each module's required counts, and a Healthy/Attention/Critical system-health rollup — composed from five already-existing read methods across Orders/Production/Inventory/Shipping/Finance, each independently try/caught so one module's failure degrades gracefully rather than breaking the whole screen. Phase B (alerts, approvals queue, recent activity, intelligence panel) was authorized once its precondition cleared — PHB-7 is now fully built (all six modules) — and reuses that work directly rather than duplicating it: alerts reuse Operations Risk's own per-module risk generators (zero new I/O, fed by data Phase A already fetches), recent activity is synthesized from bulk data already available per module (order status changes, job completions, inventory movements, shipment events, finance transactions), approvals reuse the exact bounded pattern Today's Work (PHB-5) already established (orders filtered to pending-approval status, then a small bounded loop over Customer Approval), and the intelligence panel consumes PHB-7's Sales Intelligence and Production Intelligence outputs directly. `topProducts` stays unbuilt in the intelligence panel, consistent with Sales Intelligence's own already-documented unbounded-N+1 gap, not a second one. **This closes out PHB-6 — both phases are now complete.** Full record in `Executive-Dashboard-Pre-Handover-Scope-2026-09-15.md` §8 (Phase A) and §9 (Phase B). **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** frozen's `ExecutiveDashboardService.js` builds the actual screen every employee sees on login — 7 KPI cards (Today's Sales, Monthly Revenue, Orders Today, AOV, Gross Profit, Net Profit, Cash Position), a greeting, cross-module alerts, an approvals queue, a recent-activity feed, and system-health/contract-certification status. Standalone's `dashboard-service.ts` composed only Order/Production/Inventory workspace reads plus a CRM follow-up queue before this implementation — none of the above.

**Why Tier 1:** this is first-screen, every-login content in frozen, not an optional report someone might occasionally check — the same "routine, everyday, immediately noticed" basis as Attachments and the Task Board. **Fully resolved**: every piece of frozen's actual Dashboard screen (§2.4) now has a real Standalone equivalent, including the alerts/approvals/activity/intelligence panel that Phase A had deliberately deferred.

**Scope status:** `Executive-Dashboard-Pre-Handover-Scope-2026-09-15.md`, §1–§7 (original scope), §8 (Phase A implementation record), and §9 (Phase B implementation record). A significant refinement surfaced while scoping and confirmed again during implementation: frozen's own dashboard is not backed by purpose-built executive-summary calculators for 4 of its 5 source modules (orders/inventory/shipping/finance), has **no working "efficiency" formula anywhere** for Production, and its own `criticalAlerts`/`approvalRequests`/`recentActivity` fields are essentially never populated in practice either (§9 re-confirmed this directly) — so most of this implementation's field definitions are deliberate Standalone-native choices or direct reuses of already-built PHB-7/PHB-5 logic, not frozen ports, since frozen never actually computed most of them either.

---

## Tier 2 — Real defects or open questions; not blocking day-one use, but worth resolving before or shortly after handover

### 5. PHB-4 — Order Notes (structured Comments) — ✅ IMPLEMENTED, 2026-09-15 (not yet committed)

**Status update:** authorized and implemented in this session. `GET`/`POST /orders/:orderId/notes` are built, tested, and typecheck-clean in the working tree — note row and activity-log entry written in one shared transaction, note text required (frozen's one real validation rule), and a new `orders.notes.create`/`.read` permission pair, following the precedent PHB-3 already established for `orders.attachments.*`. Frozen's own `"ON-"` ID prefix carries over unchanged — checked directly and found not to collide with anything, unlike Attachments' `"OA-"`. The `type` field stays free-text defaulting to `"Internal"`, matching frozen exactly, since no enumerated type list was found anywhere in frozen's own client code to base a fixed one on. Full record in `Order-Notes-Pre-Handover-Scope-2026-09-15.md` §8. **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** Canon names Comments alongside Attachments under Order Domain "Collaboration" (§10.4). Frozen implements a separate, typed, append-only `Order_Notes` collection (`OrderWorkspace.js`) distinct from — and in addition to — the plain scalar `Notes` field already on the Order row itself. Standalone has the scalar field, correctly, and confirmed genuinely live (editable via a textarea on the order edit form, displayed in the order detail view) — but had nothing matching frozen's separate structured collection before this implementation.

**Why Tier 2, not Tier 1 like Attachments:** the existing scalar `notes` field already provides basic remark capability today, so this was additive/enhancing rather than a capability with literally zero coverage the way Attachments was. The scope document also resolved an open question the Attachments document had left hanging: the scalar field is not a stand-in for structured Notes and was not touched, migrated, or deprecated by this implementation — frozen has both as genuinely separate things, confirmed by direct trace of both the frozen `ORDER_HEADERS` scalar column and the separate `Order_Notes` sheet, and Standalone's implementation now mirrors that.

**Full scope:** `Order-Notes-Pre-Handover-Scope-2026-09-15.md`, §1–§7 (original scope) and §8 (implementation record).

**Scope status:** written — `Order-Notes-Pre-Handover-Scope-2026-09-15.md`. Build-from-scratch, same as Attachments (no existing Standalone foundation found). Structurally near-identical to Attachments (same lock pattern, same ID-generation style, same read/write shape), differing mainly in fields and validation — implementing both together, if both are ever authorized, would likely be more efficient than solving the same small design questions twice.

### 6. PHB-7 — Per-module Executive/Business Intelligence — 🟢 FULLY IMPLEMENTED, 2026-09-16 (not yet committed)

**Status update:** authorized and implemented across four sessions, all six of six modules — Customer Intelligence (§4.5), Operations Risk (§4.6), Sales Intelligence (§4.1), Production Intelligence (§4.2), Inventory Intelligence (§4.3), and Shipping Intelligence (§4.4). All six are real, field-by-field ports of frozen's computation logic, not stubs. `reports-service.ts:1391`'s hard-coded `operationsRisk: undefined` — the bug this document's §7 flagged as "a one-line-of-consequence bug independent of the fuller build" — is fixed; that module can now connect. Sales Intelligence is reachable via its own new route (`GET /reports/sales`) rather than replacing the existing `sales` module-card reader, since that reader backs a real, currently-displayed all-time Revenue/Orders card — a different figure from Sales Intelligence's month-to-date summary — and silently changing what an existing number means was judged worse than a second route (mirroring `GET /reports/financial`). Production, Inventory, and Shipping Intelligence, by contrast, *were* wired directly into their existing `production`/`inventory`/`shipping` module-card readers, after confirming directly that each raw workspace has no `.summary` field at all — those cards were already silently falling back to a placeholder, so there was no existing meaningful number at risk. Production Intelligence also turned out not to need an Orders dependency at all, unlike frozen's own two-source design — Standalone's `ProductionJob` already carries `dueDate`/`customerName`/`priority` natively. Inventory Intelligence resolved two real data gaps Operations Risk (§8) had left unbuilt — an average-daily-usage rate and a bounded per-distinct-SKU BOM-based production-demand figure — without reopening the already-shipped Operations Risk increment. Shipping Intelligence found Standalone's `Order.expectedDispatch`/`status` fields eliminate frozen's own two-field dispatch-deadline/readiness guesswork entirely, and needs no Finance dependency at all (Standalone's Finance model has no shipping-cost/freight field for frozen's cost-override branch to read from). **This closes out PHB-7 — no sub-modules remain unbuilt.** Real data gaps in Standalone's domain model (Production has no `capacityPercent`/`utilizationPercent`/`efficiencyPercent`/`downtimeMinutes`/daily-target equivalent; Inventory Supplier has no purchase-order lead-time/on-time/spend history; Shipping has no NDR free-text reason field) were found and handled honestly rather than papered over, and one double-rounding bug in the Sales forecast (caught while hand-verifying test numbers, before it shipped) was not repeated when the same average-then-multiply pattern recurred in Production's and Shipping's own forecasts. Full record in `Executive-Business-Intelligence-Pre-Handover-Scope-2026-09-15.md` §8 (Customer Intelligence/Operations Risk), §9 (Sales Intelligence), §10 (Production Intelligence), §11 (Inventory Intelligence), and §12 (Shipping Intelligence). **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** frozen's six per-module Intelligence services (Sales, Production, Inventory, Shipping, Customer, Operations Risk) compute trend lines, top-N rankings, bottleneck/risk scoring, SLA/aging buckets, and forecasts for each domain. Standalone's `reports-service.ts` ported the module-card *shape* but not the computation before this implementation — each reader passed through raw source-module data rather than computing anything new, and the `operationsRisk` reader was hard-coded to `undefined`.

**Why Tier 2, not Tier 1 like PHB-6:** this is genuine, valuable analytics capability, but unlike the Dashboard's first-screen content (PHB-6) or the Task Board (PHB-5), it is not everyday, every-login functionality — it's a deeper reporting layer someone consults, not something that blocks or is immediately missed in ordinary daily use. Notably, Finance's equivalent (`executiveFinanceIntelligence`) has already been fully ported and is reachable via `GET /reports/financial` — proof the pattern is achievable, and now all six frozen Intelligence services join it. **Confirmed further while scoping, and worth weighing in any reprioritization**: unlike PHB-5/PHB-6, none of the six frozen Intelligence services are reachable through frozen's own live router either — each has a real, tested, dedicated view, but none of those views (nor the Hub linking them) appear in `Router.js`'s `APP.MODULES` allowlist. Frozen's own employees cannot reach this today through the app's actual navigation, which further supports Tier 2 placement over Tier 1.

**Full scope:** `Executive-Business-Intelligence-Pre-Handover-Scope-2026-09-15.md`, §1–§7 (original scope), §8 (Customer Intelligence/Operations Risk implementation record), §9 (Sales Intelligence implementation record), §10 (Production Intelligence implementation record), §11 (Inventory Intelligence implementation record), and §12 (Shipping Intelligence implementation record).

**Scope status:** written — `Executive-Business-Intelligence-Pre-Handover-Scope-2026-09-15.md`. A concrete, load-bearing detail surfaced: Standalone's `reports-service.ts` `primaryMetric()` already expects its own fixed field-name vocabulary on each module's `summary` object (e.g. `summary.revenue`/`summary.totalOrders` for sales, not frozen's own field names) — implementation should target that existing vocabulary, not reproduce frozen's naming. Also confirmed: Operations Risk (`reports-service.ts:1391`) is a one-line hard-coded-`undefined` reader that blocks that module regardless of whether its fuller computation is ever built — fixable as an independent, near-zero-cost first step. Customer Intelligence is flagged as the lowest-effort of the six to start, since its reader plumbing (`customerReader`) already exists and only needs real computation added.

### 7. CRM Lead-to-Order duplicate-conversion risk — ✅ IMPLEMENTED, 2026-09-16 (not yet committed)

**What was wrong:** `orderCreateFromLead` created the order, then separately marked the source lead converted, with no shared transaction. A failure between the two steps left the lead unmarked, and a retry would create a second, duplicate order from the same lead. Standalone reproduced this exactly, by explicit documented design (a code comment confirmed the ordering was intentional frozen-parity, not an oversight), before this fix.

**Status update:** authorized and implemented in this session, following the sequencing its own scope document laid out. `packages/platform/src/order.ts`'s `createWithoutActivity` gained an optional transaction-scoped side-effect resolver (`OrderCreateSideEffectResolver`), mirroring `updateWithoutActivity`'s existing `resolveCustomerId` pattern exactly — invoked inside the same `runInTransaction` callback, after the Order + Items inserts, before the transaction returns. `orderCreateFromLeadService` now builds a resolver that marks the source CRM lead converted through a CRM adapter bound to that same transaction runtime, replacing the old two-separately-committed-calls sequence. Order creation and lead-conversion-marking now commit or roll back together: a failure marking the lead can no longer leave an order behind whose still-unconverted lead a retry would re-convert into a duplicate order. A real architectural constraint surfaced during wiring (`server.ts` has its own test forbidding any import, even type-only, of the concrete database-runtime package) and was worked around by narrowing the runtime parameter structurally off the CRM adapter constructor's own type, rather than by naming the concrete type — full detail in the scope document's own implementation record. **Nothing has been committed or pushed.**

**Why Tier 2:** requires an actual write failure at exactly the wrong moment to trigger — not something that happens in the ordinary course of business, but the consequence (a real duplicate order reaching production/shipping) was the most customer-visible of the shared-exposure findings in this audit. Now resolved.

**Fix pattern already proven in this codebase:** Procurement's PR-to-PO conversion and GRN posting both solve this exact shape of problem correctly, using `runInTransaction` with every nested service built from the same transaction scope (verified by direct inspection, not assumed). **A second, even closer precedent was found while scoping this item and used as the actual template**: Order's own `updateWithoutActivity` already solved this identical shape of cross-domain-write problem for Customer-upsert-on-update, via an optional transaction-scoped resolver callback (`resolveCustomerId`) — `createWithoutActivity` gained the equivalent hook as part of this fix.

**Verification:** 2 new platform-layer tests (source-structure regex, matching that file's own existing convention — no live-Postgres testing exists for this adapter today, consistent with how item 8's transaction fix was also verified at the mock level) and 2 new application-service tests (resolver construction and a simulated-rollback failure path), plus fixes to 2 pre-existing architectural-boundary tests the change initially broke (a doc comment that named forbidden token strings in prose, and the `server.ts` database-import constraint above). Full monorepo build clean. No new permission key or route was needed. Full regression suite re-run: domain 339/339, platform 233/233, api 666/667 — the one failure is the same pre-existing, unrelated `reports-routes.test.mjs` timezone issue tracked since PHB-1.

**Full scope:** `CRM-Lead-Order-Duplicate-Conversion-Fix-Scope-2026-09-16.md`, §1-§7 (original scope) and §8 (implementation record).

### 8. Inventory BOM consumption partial-failure and stale-idempotency-guard — ✅ IMPLEMENTED, 2026-09-16 (not yet committed)

**What was wrong:** multi-material BOM consumption posted one inventory movement per line with no transaction wrapper; the idempotency guard checked only whether *any* consumption movement existed for the job, so a partial failure mid-BOM silently left some materials un-consumed and blocked all future retry (the guard could never distinguish "fully consumed" from "partially consumed and stuck"). Standalone reproduced this exactly, by explicit documented frozen-parity design, before this fix.

**Status update:** authorized and implemented in this session, directly (no separate scope document was written first — this item's fix was already fully specified by its own description plus a proven, already-in-the-codebase pattern to follow, unlike a from-scratch capability build). `apps/api/src/production-inventory-consumption-service.ts`'s `consumeProductionJob` now runs the idempotency check, the all-material shortage preflight, and the posting loop all inside one transaction (`transactions.runInTransaction`), reusing the existing `platformRuntime.inventoryProcurementTransactions` coordinator already used by Procurement's GRN posting and PR-to-PO conversion — the exact "proven fix pattern" this item's own text named, not a new one invented for this fix. The transaction-scoped `InventoryMovementApplicationPort` is constructed inside the callback from the coordinator's transaction-bound `materials`/`movements`/`ids`, so posting now goes through the real signed-quantity/negative-stock domain logic (previously mocked away entirely in this service's own tests, which is also fixed as a byproduct). Either every BOM line's movement posts, or none do — the idempotency guard's binary check is now always correct, since "partially consumed" can no longer exist as a persisted state. A pre-existing, now-dead `productionInventoryMovementApplication` construction in `server.ts` (the old non-transactional `movementApplication` dependency this fix replaced) was removed along with its now-unused import, rather than left as dead code. **Nothing has been committed or pushed.**

**Why Tier 2:** same shape as item 7, lower business visibility (an inventory-accounting discrepancy rather than a customer-facing duplicate order), same proven fix pattern available (the GRN service, in the same Inventory & Procurement area of the codebase, already demonstrates the correct transactional approach) — now applied here too.

**Verification:** the existing 6-test suite in `apps/api/test/production-inventory-consumption-service.test.mjs` was rewritten to mock the new `transactions.runInTransaction` shape instead of a bypassed `movementApplication`, and to assert against the real posted ledger records (including the real negative signed-quantity convention for CONSUMPTION movements — `-4.4`/`-2`, not the raw positive `4.4`/`2` the old mocked-through test only checked at the input-command level). All 6 pass, including one specifically confirming the idempotency check now runs inside the same transaction as the posting logic. Full monorepo build clean. No new permission key was needed (a pure internal-composition change, no new route). Full regression suite re-run: domain 339/339 (unchanged — no domain-layer code touched), platform 231/231 (unchanged), api 664/665 — the one failure is the same pre-existing, unrelated `reports-routes.test.mjs` timezone issue tracked since PHB-1.

**Scope status:** implemented directly; the original finding is documented in GAP-001's Domain 2 sixth pass.

### 9. Shipping-to-Order status sync — ✅ RESOLVED BY DOCUMENTATION, 2026-09-16 (no code change)

**What's different:** frozen silently swallows any failure when syncing a shipment's status onto its linked order (try/catch to console only) — the shipment update always succeeds, but Order and Shipment can silently drift apart forever with no error anywhere. Standalone has no such swallowing anywhere in the equivalent call chain (re-confirmed by direct read of `shipping-order-handoff.ts:54-87` and both call sites in `shipping-service.ts:638-643` and `:959-964` — no try/catch exists at any point in the chain) — if the sync fails there, the whole shipment operation fails and the employee sees an error.

**Resolution:** per this item's own original analysis, both branches of the "was this deliberate" question converge on the same action — keep Standalone's current behavior, with the only difference being whether to document it as an intentional decision. Since that convergence means the outcome doesn't actually depend on resolving the unknowable original intent, this closes as a documentation decision, not a design change requiring further input: **Standalone's stricter, fail-loud behavior is retained.** Failing loudly and requiring a human to notice and retry is a better default than two records silently drifting apart forever with no error anywhere frozen employees would ever see.

**A more precise picture of the actual consequence, found during this pass, worth recording exactly:** the shipment record, its timeline entry, and its event are all already persisted (`saveShipment`/`appendShipmentTimelineEntry`/`publishEvent`) *before* the Order-sync step runs in both `createShipment` and `changeShipmentStatus`. This means a sync failure reports a hard error for an operation that has, in fact, already partially succeeded — the shipment exists, only its linked Order's status wasn't updated to match. This is not a duplicate-creation risk: a retried `createShipment` correctly rejects via the existing `ACTIVE_SHIPMENT_EXISTS` guard once the first attempt's shipment row exists, and a retried `changeShipmentStatus` at the same target status passes `shippingCanChangeShipmentStatus`'s existing `noChange: true` allowance rather than being rejected. The practical effect for an employee: a "failed" shipment action they should check on (the shipment likely already exists) and, if the Order's status genuinely didn't sync, correct by hand or by retrying the status change.

**A separate, smaller, genuinely new finding surfaced while verifying the above — not part of this item's original scope, not fixed here:** `shippingCanChangeShipmentStatus`'s `noChange: true` result (`packages/domain/src/shipping.ts:422-433`, returned when `fromStatus === toStatus`) is never actually branched on anywhere in `shipping-service.ts`'s `changeShipmentStatus` — the domain layer computes it, but the application layer doesn't check it before proceeding. A retry at the same target status (e.g., after the exact partial-failure scenario above) therefore re-appends a second timeline entry and re-publishes a second event, even though the shipment's own status field doesn't change again. Low business impact (duplicate audit-trail entries, not duplicate business records), but worth a future look — not scoped or fixed as part of this item.

**Scope status:** closed by documentation; the original finding is recorded in GAP-001's Domain 5 third pass. No implementation was authorized or performed.

### 10. Finance's structural accounting model (no Journal Entry / General Ledger / Chart of Accounts) — 🟡 DECISION NOTE WRITTEN, 2026-09-16 (business decision still pending)

**What's true:** neither frozen nor Standalone implements formal double-entry bookkeeping. Both use a simpler transaction-log-plus-receivables/payables model. This is identical in both systems — not something the migration changed — and does not mean any current figure is wrong. Re-confirmed by direct read this session: `FinanceAccount` is a bare bank/cash-account list (an ID and an opening balance), not a Chart of Accounts — no Asset/Liability/Equity/Revenue/Expense classification or hierarchy exists anywhere in either codebase; the Cashbook's running balance is the closest either system comes to ledger-style bookkeeping, and that's scoped to cash only.

**Why Tier 2 and not Tier 1 or Tier 3:** whether this needs fixing at all depends entirely on GiftHatke's actual audit/compliance obligations at current scale, which is a business and accounting question this report cannot answer. It's placed in Tier 2 rather than deferred to Tier 3 only because, unlike the aspirational Canon-only gaps below, this bears on real financial reporting and deserves an explicit business decision rather than being silently left alone by default.

**Status update:** a decision-support note was written this session — `Finance-General-Ledger-Decision-Note-2026-09-16.md` — laying out the actual question, what exists today in both systems, the rough shape (not a plan) of what a "yes" answer would mean, and the questions worth asking to actually decide it. **No implementation was scoped or authorized, matching this item's own explicit caution.** Whichever way the business question is answered, that closes the item: "no" closes it with no code change (the same resolution shape as item 9, arrived at for a different, genuinely-external reason); "yes" becomes its own separately-authorized scope document, written only after the decision.

**Scope status:** implementation deliberately not scoped; a decision-support note exists (`Finance-General-Ledger-Decision-Note-2026-09-16.md`) so the underlying business question ("do we need a general ledger structure?") can actually be answered by whoever at GiftHatke is positioned to answer it.

---

## Tier 3 — Lower priority; informational, governance-only, or explicitly out of SMP1's current scope

### 11. PHB-2 — Financial Planning (Budgets, Forecasts, Scenarios, Cost Centers) — ✅ IMPLEMENTED, 2026-09-15 (not yet committed)

**Status update:** authorized and implemented in this session, all four sub-entities. `POST /finance/budgets`, `POST /finance/budgets/refresh`, and `POST /finance/forecasts` are built, tested, and typecheck-clean in the working tree. The item's previously-flagged naming-collision blocker (two different "Cost Center" concepts in frozen) turned out to already be resolved by existing code — Standalone already has both `FinanceCostCenter` and `OrganizationCostCentre` as two separate entities from an earlier implementation wave, so no fresh design decision was needed. A second correction surfaced during implementation: the scope document's claim that Scenarios/Cost Centers were "confirmed absent by direct search" was wrong — their domain types, tables, and repositories already existed (just unseeded); this item added the missing seed data and the two genuinely-missing entities (Budgets, Forecasts). A real, pre-existing bug was also found and fixed along the way: Finance's generic persistence mapper didn't normalize Postgres `Date` objects to strings (every other module's mapper already did) — this had already silently affected PHB-6's Executive Dashboard Finance KPIs, now fixed at the root. Full record in `Finance-Write-Workflows-Pre-Handover-Scope-2026-09-15.md` §9. **Nothing has been committed or pushed.**

**What's missing (original framing, for record):** real frozen capability (`FinancePlanning.js`), no Standalone equivalent before this implementation — a planning/analysis tool rather than a day-one operational blocker (nothing stops an order from being processed or a payment from being recorded by this being absent), which is why this stayed Tier 3 despite now being resolved.

**Full scope:** `Finance-Write-Workflows-Pre-Handover-Scope-2026-09-15.md`, §3 (original scope) and §9 (implementation record).

**Correction, 2026-09-21:** same as item 1 — "✅ IMPLEMENTED" described the backend only; Save Budget, Refresh Budget Actuals, and Generate Forecast had no frontend caller until 2026-09-21. See the 2026-09-21 status update below, which also documents the write-direction counterpart of this item's own already-recorded Postgres-`Date`-normalization defect: the same generic mapper normalized dates correctly on read but not on write, which is exactly what crashed on first real use.

### 12. Reorder-queue staleness (frozen-internal only)

A one-time migration moved Purchase Requisition data to a new Procurement sheet without updating the Inventory workspace's reorder-queue widget to match. This is a defect in the *frozen reference itself*, not in Standalone (Standalone's Inventory/Procurement code doesn't have the same file-level split that caused it) — nothing to implement in Standalone. Recorded for whoever owns the frozen reference, per Frozen Reference Non-Mutation; not actionable here.

### 13. Canon-corpus domain-numbering inconsistency

Documents 2 and 3 in the Canon corpus disagree with the official registry about which was certified first. A documentation/governance question for whoever holds Canon authority, not a code change.

### 14. Staff Assignment: frozen-internal defect, not actionable here

Frozen has two independent, non-communicating user-role-assignment implementations (a base family and a separate "ERP74" family) that collide on three global function names, and neither's UI is reachable through frozen's own live router. This is a frozen-reference-internal defect (name collision + dual orphaned implementations), not a Canon or Standalone gap — Standalone's actual implementation (`user-management.ts`, `security.ts`, wired REST routes) is clean, live, and arguably ahead of frozen here. Recorded for whoever owns the frozen reference, per Frozen Reference Non-Mutation; not actionable in this plan. Full evidence: GAP-001's expanded-scope sweep, Domain 25/8.

### 15. AlertService — minor, likely not employee-facing

Frozen has a dedicated `AlertService.js` (severity-classified, metrics-threshold-driven alert records) with no Standalone equivalent located. Its only caller in frozen is an internal runtime-performance measurement, and it is not located in any frozen HTML client UI — this reads as an internal/diagnostic capability rather than something employees would notice missing. Not registered as a blocker; flagged for completeness. Full evidence: GAP-001's expanded-scope sweep, Domain 9.

### 16. Aspirational Canon-only gaps (no implementation exists in either codebase)

Master Data Management as a stewardship layer, enterprise Search, AI/Decision Intelligence, Legal/Contract/Compliance tracking beyond tax, Asset/Facility management, Sustainability/EHS, Data Governance, formal Scheduling, Product/Catalog as an independent master-data domain, and most of Canon Domains 29–44. None of these have any implementation in frozen either (re-confirmed against the full 484-file frozen repo listing, not just the original survey pass) — Standalone didn't fail to build something frozen has; neither system ever built these. Two of these are explicitly Canon-confirmed **out of scope for SMP1 specifically** (AI Agents per Domain 42's own Canon text; public marketplace/partner portals per Domain 43's, which also independently confirms the Reseller/Partner Dashboard wave is correctly sequenced as future work, not current scope). Not actionable pre-handover; not recommended for this implementation plan at all.

**Correction, 2026-09-15:** this item previously also listed Quality Management and "a generic Workflow/Business-Process engine" as aspirational Canon-only gaps. Quality Management was re-verified and found to have genuine, full parity — it's implemented as a QC checklist embedded in Production on both sides, not an independent domain (see GAP-001's expanded-scope sweep, Domain 4); it was never a gap and should not have been listed here. The generic Workflow/Business-Process engine claim holds narrowly (neither system has an actually-adopted generic engine — frozen's own `WorkflowService.js` has one live caller and one registered entity type), but a closely related capability was incorrectly swept into the same "aspirational" bucket: frozen's `TaskService.js`/`WorkService.js` "Today's Work" task board is real, live, UI-wired, employee-facing functionality with no Standalone equivalent. That capability is now separately registered as Tier 1 item 3 (PHB-5), not listed here. See GAP-001's expanded-scope sweep, Domain 11, for full detail on both corrections.

---

## Cross-reference: items from before this session, still open, not covered by this plan

These were already known and open at the start of this continuation session's takeover, are not GAP-001 findings, and are not re-scoped here — listed only so this consolidated plan doesn't read as a complete pre-handover checklist on its own:

- **GAP-002** — production secret custody. **Closed, 2026-09-21**: operator decided Render's own environment-variable storage is sufficient, no dedicated secrets-management provider needed. `smp1-gap-002-secret-custody-decision-2026-09-21.md` (Standalone repo).
- **GAP-009** — Render/Neon topology, backup, restore, rollback, and operational-ownership certificate. Not touched by this session as a whole, but the expanded-scope domain sweep surfaced two observations relevant to whoever owns it: Standalone's `GET /health` endpoint currently returns a hardcoded constant regardless of real system state (a real Postgres health probe exists in `packages/database/src/health.ts` but is not wired into it, as read), and Standalone's more capable diagnostics and backup/recovery application services are both explicitly instantiated and then deliberately left unpublished (`apps/api/src/server.ts`, by its own code comments). This may be entirely intentional for the current phase — this report cannot determine intent from source code — but is worth this GAP's owner knowing about. Not registered as a GAP-001 pre-handover blocker, since it's operational/infrastructure posture rather than a Canon business-domain capability.
- **GAP-011** — operator-side clean-worktree and frozen-HEAD attestation. **Closed, 2026-09-21**: this session runs directly on the operator's local machine with both `$HOME/GiftHatkeOS-Standalone` and `$HOME/GiftHatkeOS` mounted — the earlier blocker ("the cloud runtime cannot mount" either path) no longer applies. Standalone's local checkout is clean and current with `origin/smp1/production-parity` at `3e28cc0`; the frozen reference's local checkout sits exactly at the certified immutable tag `v3.9.10-APPS-SCRIPT-V1.0-PERMANENT-REFERENCE-FREEZE` (`fd7c754`), independently verified identical to the remote tag object via `git ls-remote`, with no tracked-content change of any kind (only a `.DS_Store` binary rewrite). `smp1-gap-011-worktree-frozen-head-attestation-2026-09-21.md` (Standalone repo).
- **Phase 3 of the Handover Roadmap** — fresh authenticated live acceptance testing across all employee workspaces. Not performed in this session (this report's mandate throughout has been read-only source review).

## What this document does not do

It does not implement anything. It does not authorize implementation of anything. It does not alter Finance's, Orders', CRM's, Procurement's, Inventory's, Production's, or Shipping's existing certified/closed statuses. It does not reopen GAP-001 in a new way — GAP-001 remains open and blocking exactly as it already was, for Domain 1's still-incomplete field-level closure and for every domain beyond the first two that has not received Domain-1-level depth. This document exists to make the next authorization decision easier, not to make it.

## Status update, 2026-09-21 — commit status, and this document's role going forward

**Every "(not yet committed)" marker throughout this document (all 16 items) is
now stale and should be read as historical, not current.** All items were
committed on 2026-09-16 (`50fd3a3` — "feat(smp1): close GAP-001 consolidated
pre-handover implementation plan") and pushed to
`origin/smp1/production-parity`. A further fix (item 8's Kysely migration
type-string defect, hit only when the pending migrations were actually run
against live Postgres for the first time) was committed separately
(`14bf97e`) and is also pushed. Both commits, all five new migrations, and
the resulting deploy were confirmed working end-to-end by a fresh
authenticated acceptance pass (`Phase-3-Fresh-Authenticated-ERP-Acceptance-
2026-09-16.md`) — so "Phase 3 of the Handover Roadmap... Not performed in
this session," in the cross-reference section above, is also stale; Phase 3
completed 2026-09-16. GAP-009's cross-reference entry above is partially
stale too: its documentation half is now addressed by
`smp1-standalone-v1-render-neon-production-topology-2026-09-21.md`; its
operational half (live backup/restore, monitoring) remains open exactly as
described above.

**This document is now designated the ongoing closure record for the eleven
capabilities `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §2
found had zero certification/closure record** (Item 7, Item 8, and PHB-1
through PHB-7's sub-items, all covered by name above). No separate
per-capability certification document will be written for these — each
item's own section above already carries implementation detail, verification
method, and test counts comparable to or exceeding what a dedicated
certification document in this repository's existing pattern (e.g. the
Production-Inventory-Consumption certification series) would add, and a
second parallel record would only create two documentation tracks that could
silently drift apart — the exact risk already found and corrected once this
session, in GAP-001's own identity being silently reused for a different
meaning between 2026-09-13 and 2026-09-14 (see
`smp1-finding-register-reconciliation-2026-09-13.md`'s 2026-09-16 correction).
Full reasoning: `docs/governance/smp1-per-capability-certification-retirement-
2026-09-21.md` (Standalone repo).

This does not certify any of the eleven items as complete in the sense the
existing per-capability certification pattern uses that word — it designates
where their record of record now lives. Domain-level Canon parity for the
domains these items touch (Finance, Inventory/Procurement, Production,
Orders, Shipping) remains governed by GAP-001's own broader open status and
the 44-domain parity matrix, unchanged by this update.

## Status update, 2026-09-21 (continued) — Finance and Inventory Materials employee-facing mutation UI, and a Finance persistence defect found and fixed

**Trigger:** the operator asked directly why the Inventory and Finance
workspaces were still read-only in Employee Web. Investigation (not
assumption) found two different situations sharing one root cause:

- **Finance** was a pure read-only summary view (`apps/web/src/finance.ts`
  before this update): metric cards and record counts, zero forms, zero
  mutation calls. All seven of PHB-1's and PHB-2's backend routes (item 1,
  item 11 above) existed, were permission-gated and CSRF-protected, and had
  no caller anywhere in the frontend.
- **Inventory** was only partially read-only. Procurement (Purchase
  Requisition → Purchase Order → Goods Receipt) was already fully built and
  wired with real forms (`apps/web/src/inventory-procurement.ts`'s existing
  modal infrastructure) — not a gap. Material create/update was the actual
  gap: `POST /inventory/materials` and `PATCH /inventory/materials/:materialId`
  (Slice 7C/7D, certified and live per this repository's own Inventory
  Material certification series in `docs/governance/`) had no frontend
  caller either, for the same reason as Finance — the backend shipped ahead
  of any UI to reach it.

Both are the same shape of gap items 1 and 11 already describe: "✅
IMPLEMENTED" in this plan meant the backend was implemented, not that an
employee could reach it. That distinction was accurate at the time (this
plan's own scope was orchestration/HTTP, per item 1's "the orchestration
layer and HTTP routes did not [exist]") but reads as a stronger completeness
claim than intended once no frontend followed. Corrections were added to
items 1 and 11 above pointing here.

**What was built:**

- **Inventory Materials** — a new CSRF-aware mutation client
  (`apps/web/src/inventory-materials-mutation-api.ts`) and Create Material /
  per-row Edit Material forms, wired into `inventory-procurement.ts`'s
  existing Procurement modal (`openProcurementModal`/`data-procurement-kind`
  submit-dispatch pattern already proven for PR/PO/GRN — reused, not
  duplicated). Covers every field the backend's `InventoryMaterialMutation-
  Fields` accepts (SKU, name, category, unit, supplier, size, color, base
  price, GST%, transport charges, reorder level/quantity, location, active
  status, opening stock on create only). Size and color are submitted as
  opaque free text (`dimensionMode: "Custom"`) rather than reproducing the
  backend's dimension-mode/unit composition logic client-side — a
  deliberate simplification, not a missed requirement; the backend already
  treats "Custom" mode as pass-through free text. The stale "Read only"
  toolbar badge (inaccurate since Procurement was already writable) was
  removed.
- **Finance** — a new CSRF-aware mutation client
  (`apps/web/src/finance-mutation-api.ts`) and a full rebuild of
  `finance.ts`'s render path to add real forms for all seven actions: Record
  Receipt, Record Expense, Pay Expense, Reverse Transaction, Save Budget,
  Refresh Budget Actuals, Generate Forecast. Account ID / category / cost
  center fields are plain text inputs with a "known values" hint line
  sourced from the live workspace snapshot (e.g. `Known values: RAW_MATERIALS,
  PACKAGING, SHIPPING, MARKETING, UTILITIES` for expense category) rather
  than a dropdown bound to an assumed key name — the backend treats these as
  free-form identifiers, and the read-side `FinanceWorkspaceResponse` type
  is intentionally untyped per-section (`Record<string, readonly unknown[]>`),
  so a rigid dropdown would have been guessing at a schema the code doesn't
  actually commit to.

**A real defect found during live verification, not in the original scope
(same shape as item 1's period-lock mismatch and item 11's Postgres-`Date`
read-normalization bug — a real, previously-untriggered defect surfaced by
being the first real caller):** creating a brand-new expense or receivable
sets `invoiceDate`/`dueDate` (payables, receivables) or `approvedAt`
(expenses) to `""` — the domain layer's established "no value yet" sentinel,
used consistently elsewhere. All three columns are Postgres `timestamp`
columns (confirmed against `migration/20260911130000_finance_foundation.mjs`),
and the generic Finance persistence mapper
(`financeRecordToPersistence`/`createPlatformFinanceRepositoryAdapter` in
`packages/platform/src/finance.ts` and `finance-repositories.ts`) wrote that
`""` straight through with no translation. Postgres rejects an empty string
for a `timestamp` column outright. `POST /finance/receipts` and `POST
/finance/expenses` both 500'd on their very first real call — reachable
because Finance had zero frontend callers until this update, exactly
mirroring item 11's own already-recorded finding that the same shared
mapper's *read*-direction Date-normalization gap "had already silently
affected PHB-6's Executive Dashboard Finance KPIs" before being caught.
Today's defect is that same mapper's *write*-direction counterpart, not a
new class of bug.

**Fix:** `createPlatformFinanceRepositoryAdapter` gained an optional
`nullableTimestampColumns` list, applied in `save()`/`update()` immediately
before every write — converts `""` to `null` for exactly the named columns,
leaving every other field (including legitimate empty-string `TEXT` columns
like `notes`) untouched, and leaving the domain layer's `string`-typed
`invoiceDate`/`dueDate`/`approvedAt` fields and their `""` convention
unchanged everywhere else. Wired for `finance_payables`/`finance_receivables`
(`invoice_date`, `due_date`) and `finance_expenses` (`approved_at`), in both
the transactional repository construction (`finance-money.ts`, the path the
live 500s actually went through) and the non-transactional one
(`finance-repositories.ts`). Matches the `nullableDateValue` precedent
already established in `packages/platform/src/shipping.ts` — convert at the
specific write call site, not by making the generic mapper guess at column
types.

**Explicitly out of scope, flagged rather than silently built or silently
skipped:** manual inventory stock-movement/adjustment (correcting a stock
count, writing off damaged/lost stock, recording a return outside
Procurement). Confirmed by exhaustive route enumeration
(`grep -rn '"/inventory' apps/api/src/routes/*.ts`) that no such route
exists anywhere — stock only ever changes via Material creation's
`openingStock` field (create-only, frozen behavior) or the Procurement
receipt flow. This is a genuine backend capability gap, not a missing
frontend, and was not built. The operator was informed directly and asked
that it be tracked as a follow-up rather than implemented in this pass.

**Verification:** full monorepo build and typecheck clean across all five
workspaces. 4 new platform-layer tests added
(`packages/platform/test/finance-repositories.test.mjs`) covering the new
`nullableTimestampColumns` behavior directly, including one exercising
`createPlatformFinanceRepositoryAdapter.save()` end-to-end against a mocked
Kysely `insertInto`/`onConflict` chain. Full regression suite re-run: domain
339/339, platform 237/237 (233 pre-existing + 4 new), web 103/103, api
666/667, database 224/225 — the two failures are both pre-existing and
unrelated to this work: the same `reports-routes.test.mjs` date-drift issue
this plan's cross-reference section already tracks as GAP-004, and a stale
expected-migration-list snapshot in `packages/database/test/migration-
foundation.test.mjs` that has no connection to Finance or Inventory. One web
test (`inventory-procurement-read-workspace.test.mjs`) initially failed
after the Inventory change — not a real regression, but a local variable
named `createMaterialButton` incidentally containing the literal substring
`"createMaterial"`, which a frozen-scope guard test checks for by design;
renamed to `newMaterialButton` and the test passed cleanly on its own terms
(the guard's actual purpose — keeping `postMovement`/production-consumption
wiring out of this workspace — remains correctly enforced, since neither was
built here).

Live-verified in the browser as `support.gifthatke@gmail.com` against
`erp.gifthatke.in` after each deploy: created a Material end-to-end (SKU/
name/pricing/opening stock all persisted correctly) and edited it (fields
pre-filled correctly from the live record, edit saved). Record Expense with
Payment Status "Paid" correctly hit a real business-rule 409 ("no financial
period is configured [as Open] for the current month") rather than any kind
of application error — an environment/data-setup matter for whoever owns
Finance's period configuration, not a code defect. Record Expense with
Payment Status "Pending" 500'd before the fix and saved successfully after
it. Record Receipt against a deliberately fake order ID correctly hit a real
404 ("order not found") after the fix, proving the request now reaches real
business logic rather than crashing on the write path.

**Commit and deploy record:** three commits on `smp1/production-parity`,
pushed to `origin`:

- `29074b2` — `feat(inventory): add Create/Edit Material forms to
  Procurement workspace`
- `bb2ce43` — `feat(finance): add employee mutation forms to Finance
  workspace`
- `6362245` — `fix(finance): stop empty-date sentinels from crashing new
  payables/receivables/expenses`

Both Render services were redeployed and confirmed live: the static Web
service (`srv-da4ev9rncjis73fcrmhg`) after the first two commits, the API
service (`srv-da4ev9rncjis73fcrmh0`) after the third.

**Why this belongs in this document rather than a new one:** same reasoning
`docs/governance/smp1-per-capability-certification-retirement-2026-09-21.md`
already gave for the original eleven items — this is a direct continuation
of items 1 and 11's own work, found and closed in one session, with full
implementation detail and verification already recorded here. A separate
document would restate this, not add to it.

**What remains open after this update:** manual stock-movement/adjustment
(above); GAP-001's own domain-by-domain Canon reconciliation, unchanged by
this update; GAP-004 (Reports timezone sensitivity, unchanged, still the
same failing test); item 10 above (Finance General Ledger / Chart of
Accounts business decision, unchanged, still pending).
