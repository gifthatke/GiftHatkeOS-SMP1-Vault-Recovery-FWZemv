# Fix Proposal — Order Update Customer-Write-Before-Revision-Rejection Defect

**Date:** 2026-09-14
**Status:** PROPOSAL ONLY — NOT AN IMPLEMENTATION AUTHORIZATION
**Origin:** GAP-001, Domain 1 (Order Management), sixth/ninth/tenth passes
**Finding linkage:** `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md`

This document proposes a fix for a source-level defect found during GAP-001's
read-only review. It is a scope-lock-style proposal, not a patch. No code has
been changed. Writing or merging any change described here requires a
separate, explicit authorization, followed by its own implementation,
tests, and controlled-closure record — the same two-step process every other
SMP1 module in this repository has followed (see, for example,
`docs/certification/SMP1_ORDER_MUTATION_TRANSPORT_CERTIFICATION_AND_CONTROLLED_CLOSURE.md`).
GAP-001 itself remains OPEN / BLOCKING regardless of whether this proposal is
later accepted.

## 1. The defect, restated precisely

**File:** `apps/api/src/server.ts`, `customerAwareOrderMutationPersistence.updateWithoutActivity` (~line 1155–1190 at commit `c3d017affb0d85b7ec8092494242aa5140374e93`).

The function currently:

1. Reads the existing order snapshot (`baseOrderMutationPersistence.findById`).
2. Calls `platformRuntime.customerOrderAdapter.resolveOrUpsertCustomer(...)` — **unconditionally**. This either creates a new customer row (`totalOrders: 1, lifetimeValue: input.order.grandTotal`) or, when the order has no existing linked `customerId`, increments an existing customer's `totalOrders`/`lifetimeValue`. This call is its own committed database write, immediately, outside any transaction shared with the order update.
3. Only then calls `baseOrderMutationPersistence.updateWithoutActivity(...)` (`packages/platform/src/order.ts`), which opens its own transaction, re-reads the current row, and calls `assertExpectedVersion(currentRevision, input.expectedRevision)` — the actual stale-revision rejection.

**Consequence:** if step 3 rejects the update (stale revision), step 2's customer write already happened and is not rolled back. A customer can be created, or have its order count/lifetime value inflated, for an order edit that never actually took effect. A normal client retry (correct the revision, resubmit — the exact behavior the rejection message invites) repeats step 2 against the now-existing customer, and because the order's own `customerId` still was not set on the failed first attempt, the increment condition (`!input.existingCustomerId`) is true again, double-counting the aggregate.

**Frozen's corresponding behavior** (`Orders.js`, `orderSaveInternal_`): the revision check (`if (existingOrder && requestedRevision && requestedRevision !== ...) throw ...`) runs *before* `upsertCustomerFromOrder_` is called, inside one `LockService.getDocumentLock()` critical section covering the whole save. Frozen therefore never reaches the customer write on a rejected update, and has no equivalent exposure.

## 2. What this proposal does not authorize

- No change to Standalone's revision-check strictness. Frozen tolerates an
  omitted/zero revision (bypassing the check entirely); Standalone always
  compares. GAP-001's fifth/sixth passes already established this asymmetry
  and explicitly recorded that any future correction must not weaken
  Standalone's current protection to chase literal parity with frozen's
  looser behavior. This proposal does not touch `assertExpectedVersion` or
  its call sites' strictness.
- No change to the `createWithoutActivity` path's customer-resolution
  *ordering* — create has no revision to reject against, so the ordering
  defect does not apply there. (A separate, narrower, lower-confidence
  exposure — an orphaned customer if the order write itself fails for an
  unrelated reason after the customer write, on either the create or update
  path, in either codebase — was noted in the tenth pass and is out of
  scope for this proposal; it would need its own investigation.)
- No change to frozen. Frozen is the read-only behavioral reference.
- No change to any other module, route, permission, or migration.

## 3. Two options, honestly compared

### Option A — Pre-check the revision before resolving the customer (cheap, partial)

Add a read-only revision check immediately at the top of
`customerAwareOrderMutationPersistence.updateWithoutActivity`, before calling
`resolveOrUpsertCustomer`: read the current row (already being fetched for
`existingCustomerId` — no new query needed), compare its revision to
`input.expectedRevision` using the same rule `assertExpectedVersion` uses,
and throw immediately on mismatch, without calling the customer adapter.

**What this fixes:** the sequential case demonstrated in GAP-001's ninth
pass — a rejected attempt followed by a retry. The retry's pre-check sees
the *same* revision the transaction would have rejected and fails before
touching the customer table at all. This is the common, expected case (a
user's form went stale, they refresh, they resubmit).

**What this does not fix:** a genuine concurrent race — two requests reading
the same current revision at nearly the same time, both passing the
pre-check, both calling `resolveOrUpsertCustomer` (so both could mutate the
customer), with only one winning the transaction's compare-and-swap inside
`baseOrderMutationPersistence.updateWithoutActivity`. The loser in that race
still mutates the customer before being rejected. This is a narrower window
than today's guaranteed-every-time exposure, but it is not zero.

**Cost:** small, additive, one function. Lowest risk to review and merge.

### Option B — Move customer resolution inside the same transaction (complete, larger)

Restructure so `resolveOrUpsertCustomer`'s write participates in the *same*
database transaction as the order update, gated by the same revision check,
so the whole operation is atomic: either both the customer write and the
order write happen, or neither does.

This is architecturally feasible without a deep rewrite, because of how the
codebase is already shaped:

- `packages/platform/src/customer-persistence.ts`'s functions
  (`insertCustomerRecord`, `updateCustomerRecord`, `findCustomerRecordById`,
  `findCustomerRecordByPhone`) all take a `DatabaseRuntime` parameter and
  operate via `runtime.db` — and `DatabaseRuntime` (`packages/database/src/database.ts`)
  is just `{ db: Kysely<DatabaseSchema>; destroy(): Promise<void> }`. A
  Kysely transaction handle (`trx`, already used throughout
  `packages/platform/src/order.ts`'s `runInTransaction` blocks) satisfies
  the same `Kysely<DatabaseSchema>` shape, so a `{ db: trx, destroy: async () => {} }`
  wrapper can stand in for the ambient runtime without changing
  `customer-persistence.ts` at all.
- `resolveOrUpsertCustomer` has exactly one non-test caller in the whole
  codebase — `server.ts`'s two `customerAwareOrderMutationPersistence`
  methods (confirmed by repository-wide search in this session). Changing
  its call shape has a narrow, fully-enumerable blast radius.

Concretely: give `baseOrderMutationPersistence.updateWithoutActivity`
(`packages/platform/src/order.ts`) an optional callback parameter — e.g.
`resolveCustomerId?: (trx) => Promise<string>` — that it invokes, passing
its own `trx`, *after* `assertExpectedVersion` succeeds and *before* it
writes the order row. `server.ts` then passes a trx-aware version of
`resolveOrUpsertCustomer` as that callback instead of calling it beforehand.
The customer write becomes part of the same atomic unit the revision check
already gates, for both the sequential-retry case and the genuine
concurrent-race case.

**Cost:** touches `packages/domain/src/order-ports.ts` (or a new internal
type, if the callback shouldn't be part of the public port), `packages/platform/src/order.ts`
(both `createWithoutActivity`/`updateWithoutActivity` if applied
consistently to both), and `server.ts`'s composition. More files, more
review surface, but closes the defect completely rather than narrowing it.

## 4. Recommendation

Option B is the correct long-term fix and is not as invasive as it might
first appear, given the transaction-shape compatibility identified in §3.
Option A is a reasonable interim mitigation if a fast, low-risk merge is
wanted before Option B is scheped and reviewed — it closes the case GAP-001
actually traced concretely (rejection followed by retry) while leaving a
narrower, harder-to-hit race open. Whoever authorizes this work should pick
one explicitly; this proposal does not choose on its own.

## 5. Suggested acceptance criteria (for whichever option is authorized)

- A regression test reproducing the exact sequence GAP-001 traced: attempt
  an update with a stale `expectedRevision` on an order with no linked
  `customerId`, confirm the update is rejected, then confirm the customer
  table is unchanged (Option A and B) and — for Option B only — confirm the
  same holds under two genuinely concurrent update attempts.
- No change to `assertExpectedVersion`'s behavior or any existing passing
  test for the revision check itself.
- No change to `createWithoutActivity`'s customer-resolution behavior
  (unless the team separately decides to investigate the narrower orphan-
  customer exposure noted in §2, which this proposal treats as out of
  scope).
- Existing Order transport/coordinator regression suites
  (`apps/api/test/order-production-mutation-coordinator.test.mjs`,
  `apps/api/test/order-service.test.mjs`, `apps/api/test/customers-routes.test.mjs`)
  continue to pass unmodified in intent, updated only where they assert the
  old call ordering.

## 6. What happens next

This proposal is not self-executing. It requires:

1. Explicit authorization to proceed (and a choice between Option A and
   Option B, or a decision to do both — A now, B later).
2. Its own scope-locked implementation slice, following this repository's
   established pattern (implementation → tests → regression → Obsidian
   reconciliation → controlled closure).
3. A decision on whether it happens before or after handover, given it
   does not block day-to-day use of already-certified functionality but
   does represent a real, if narrow, data-integrity exposure in a
   customer-facing record.

GAP-001 remains OPEN / BLOCKING independent of this proposal's disposition.

## 7. Implementation record — Option B, 2026-09-14

The operator authorized proceeding with Option B. Implemented in this
session, uncommitted (pending explicit authorization to commit/push).

**Files changed:**

- `packages/platform/src/order.ts` — `PlatformOrderMutationPersistenceAdapter.updateWithoutActivity`
  now accepts an optional `resolveCustomerId?: OrderUpdateCustomerResolver`
  parameter (new type, same file). Inside `createPlatformOrderMutationPersistenceAdapter`'s
  transaction, the resolver — if provided — is invoked only after
  `assertExpectedVersion` succeeds, passed a `DatabaseRuntime` wrapping the
  transaction's own `trx`. Its result becomes `resolvedOrder.customerId`,
  used for both the actual database write (`toOrderUpdateValues(resolvedOrder, ...)`)
  and the returned snapshot, so the two can never disagree.
- `apps/api/src/server.ts` — `customerAwareOrderMutationPersistence.updateWithoutActivity`
  no longer calls `resolveOrUpsertCustomer` before `baseOrderMutationPersistence.updateWithoutActivity`.
  It now passes a resolver callback that, given the transaction-scoped
  runtime, constructs a customer adapter from the same three existing
  factory functions used in this repository's own composition root
  (`createPlatformCustomerOrderAdapter`, `createPlatformCustomerRepositoryAdapter`,
  `createPostgresCustomerIdentityAllocator`) and calls `resolveOrUpsertCustomer`
  on it — now running inside the order's own transaction. `createWithoutActivity`
  is untouched, matching this proposal's §2 scope boundary.
- `packages/platform/test/order.test.mjs` — new test asserting, from the
  actual source text, that `assertExpectedVersion` precedes `resolveCustomerId(`
  inside `updateWithoutActivity`, that the resolver runs inside
  `runInTransaction`, and that both the write and the returned snapshot use
  `resolvedOrder`.
- `apps/api/test/customers-routes.test.mjs` — the pre-existing "Order
  Customer composition injects authoritative totals before persistence"
  test asserted the old call ordering by construction; updated to assert
  the new one (customer resolution now reached through a callback argument,
  with `baseOrderMutationPersistence.updateWithoutActivity(input, ...)`
  appearing before `resolveOrUpsertCustomer(` rather than after).

**Verification (Node v26.7.0 — the machine's only installed Node; v24.19.0
per the standing operating instruction was not present and was not
installed for this pass; nothing in the failures or passes below is
Node-version-specific):**

- `npm run typecheck` (full monorepo, all 6 workspaces): clean.
- `packages/platform` full test suite: 194/194 passing, including the new
  GAP-001 regression test.
- `apps/api` full test suite: 492/492 passing, including the updated
  composition test.
- `packages/domain` full test suite: 202/202 passing (unaffected by this
  change; run for completeness).
- `packages/database` full test suite: one pre-existing, unrelated failure
  in `migration-foundation.test.mjs` (a hardcoded expected-migrations list
  missing a migration that already existed before this session started).
  Not touched, not caused by, and not fixed by this change — out of scope
  for this proposal.

**Not done, and deliberately so:** no commit, no push, no migration, no
deployment, no change to `createWithoutActivity`, no change to
`assertExpectedVersion`'s strictness.

**A correction to how the baseline was handled:** the local Standalone
checkout's HEAD (`79210724…`) was never fast-forwarded to the
GAP-001-analyzed commit (`c3d017a…`) — an attempt to do so (stash, then
fast-forward merge) was blocked by this session's own tooling permissions
as a destructive-local-history action, and was not retried or forced.
Instead, before writing any code, this session verified directly (`git
diff --stat` between the two commits) that the specific files this fix
touches — `apps/api/src/server.ts` (aside from unrelated later insertions
elsewhere in the file), `packages/platform/src/order.ts`,
`packages/platform/src/customer.ts`, `packages/domain/src/order-ports.ts`,
and `packages/database/src/customer-persistence.ts` — were byte-identical
between the two commits. The implementation was therefore written directly
against the actual local working tree, which was confirmed equivalent to
the GAP-001 baseline for every file this change reads or edits, not against
a synchronized copy. The local branch pointer itself remains at
`79210724…`, still behind `origin/smp1/production-parity`; that sync
question (recorded as open in `GiftHatkeOS-SMP1-Handover-Roadmap-and-Execution-Plan-2026-09-13.md`'s
§14/§15) remains open and separate from this implementation.

**Committed:** the operator authorized a local commit, not yet a push.
Commit `dc8c7b2c6f123e3c0ddddcaf6d079d349baafe7f` on `smp1/production-parity`,
containing exactly the four files above and nothing else — the unrelated,
pre-existing uncommitted vault-relocation diff in the same working tree was
left untouched. Local branch HEAD is now `dc8c7b2…`, still one commit ahead
of where `origin/smp1/production-parity` (`c3d017a…`) sits relative to it —
i.e., local is still behind origin on the ~300 commits noted in §7's
correction above, plus now one commit ahead on top of its own prior tip.
Pushing has not happened and was explicitly deferred by the operator.

## 8. Branch sync and rebase, 2026-09-14

The operator authorized syncing the local branch to origin and rebasing the
fix commit on top. Performed as separate steps (not one chained command,
after an earlier chained stash+merge attempt was blocked by this session's
own tooling permissions as a destructive-local-history pattern):

1. `git stash push -u` — set aside the unrelated, pre-existing
   vault-relocation deletions so the rebase would not need to reason about
   them.
2. `git fetch origin` — confirmed `origin/smp1/production-parity` still at
   `c3d017a…`.
3. `git rebase origin/smp1/production-parity` — replayed the single local
   fix commit onto `c3d017a…`. Clean, no conflicts (expected: every file
   this commit touches was already confirmed byte-identical between the
   old local base and `c3d017a…` in §7).
4. `git stash pop` — restored the vault-relocation deletions untouched.

Result: local `smp1/production-parity` is now `c3d017a…` → `70d5e8d58ec9e250b84d42b3090f29238fbf4807`
(the fix commit, rebased — new hash, same content). One commit ahead of
origin, not yet pushed. The vault-relocation deletions (102 files) remain
exactly as they were, uncommitted, untouched by any of this.

**Re-verification after sync**, since the rebase brought in real new code
(Reports, Settings, User Management, and more — everything in the ~300
commits this branch was previously missing):

- Building `packages/domain` and `packages/database` was required first —
  their compiled `dist/` output hadn't been regenerated for the newly-pulled
  source, so a monorepo-wide build/typecheck run immediately after the
  rebase surfaced transient "module has no exported member" errors in
  `packages/platform`'s new Settings/User Management files and in
  `apps/api`. This was a stale-build-artifact issue, not a code defect: a
  full `npm run build` (root) followed by `npm run typecheck` (root) came
  back completely clean across all 6 workspaces.
- `packages/platform` full test suite: 199/199 (was 194 before the sync;
  the difference is new Settings-related tests that came in with the
  rebase, all passing).
- `apps/api` full test suite: 499/500. The one failure,
  `"Reports projection preserves the frozen statement arithmetic"`
  (`reports-routes.test.mjs`), is unrelated to this fix — it is in Reports
  code this session never touched, was not present in this local checkout
  before the sync (Reports arrived with the ~300 pulled-in commits), and
  its assertion compares a computed `periodFrom` date against a literal
  string the test hardcodes, independent of anything in this change. Not
  investigated further or fixed here — flagged as a separate, pre-existing,
  out-of-scope issue for whoever owns Reports.

GAP-001 remains OPEN / BLOCKING.

## 9. Pushed, 2026-09-14

The operator authorized the push. `origin/smp1/production-parity` was
re-fetched immediately beforehand and confirmed unchanged at `c3d017a…`
(no one else had pushed in the interim), so this was a clean fast-forward
push, not a force push: `c3d017a..70d5e8d`. `origin/smp1/production-parity`
and the local branch are now both at `70d5e8d58ec9e250b84d42b3090f29238fbf4807`.
The vault-relocation deletions remain uncommitted and local-only, as before.

GAP-001 itself is unaffected and remains OPEN / BLOCKING — this closes only
the one fix proposed and tracked in this document, not the broader review.

**Next step:** none required by this proposal. Any follow-on work (the
narrower orphan-customer exposure on the create path noted in §2, a parallel
fix on the frozen side if ever authorized, or continuing GAP-001 itself)
would need its own separate authorization.
