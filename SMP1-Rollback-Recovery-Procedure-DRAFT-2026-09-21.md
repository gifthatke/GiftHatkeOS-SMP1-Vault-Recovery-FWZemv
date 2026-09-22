# SMP1 Standalone — Rollback and Recovery Procedure

**Date:** 2026-09-21
**Status:** DRAFT PROPOSAL — NOT AUTHORIZED, NOT CERTIFIED. Written to close a genuine gap in Phase 4's evidence pack (no rollback/recovery procedure existed anywhere in the corpus, distinct from the already-documented backup/recovery *metadata model*). Starting point for the operator to correct.
**Origin:** `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §4.

## What the 2026-09-21 drill verified, and what it didn't

A real restore drill was run against a disposable Neon branch on 2026-09-21 (not production — nothing below touched live data). **The core mechanism works and is fast; the real constraint is how far back it reaches, not whether it functions.**

**Verified, directly, in the Neon console:**

- **Plan and retention are now known facts, not assumptions**: this project is on Neon's **Free plan**, with a **6-hour** point-in-time history window (previously this draft only assumed PITR existed at all).
- **The 6-hour boundary is real and enforced, not just a label.** Attempting to branch from a point 24 hours back was rejected with an explicit, precise error: *"Date is beyond the history retention. The earliest available point is Sep 21, 2026 5:10 am"* — consistent to the minute with the stated 6-hour window at drill time (11:09/11:13 am).
- **Branch creation from a past point in time works, is fast (~3 seconds), and produces a real, fully queryable copy.** A branch created from a point 3 hours back was spot-checked with a live SQL query: order count matched production exactly (1), confirming the copy is genuine, not a stub.
- **There's a dedicated, non-CLI path for this**: the Neon console's "Backup & Restore" → "Restore from history" panel does exactly this through a UI — pick a source branch and a timestamp, restore or branch from it. Whoever ends up owning this procedure doesn't need `pg_dump`/`psql` comfort to at least get a historical copy.
- **A new gap, not previously known**: that same panel's "Preview data" feature (meant to browse/query data at a past point without first creating a branch) failed consistently on two attempts — *"Error connecting to database: signal is aborted without reason"*, same error ID both times. Not a blocker (creating a branch and querying it, as done in this drill, works reliably as a substitute), but worth knowing before depending on "Preview data" specifically in a real incident.
- **Also new**: Neon has a separate, longer-lived "snapshot" feature (schedulable point-in-time saves, independent of the rolling PITR window) — but it is **not configured**: *"No snapshots, no schedule set."* This means recovery capability today is bounded strictly to the rolling 6-hour window. If a problem isn't caught within 6 hours of it happening, there is currently no way to recover the prior state through Neon's own tooling at all.
- Drill branches were created and deleted cleanly; no residue left in the project (back to 1/10 branches after cleanup).

**Not verified by this drill — still a real gap in §2 below:**

- **Actually cutting production traffic over to a restored branch was not tested.** The drill proved a correct historical copy can be created and read; it did not test making that copy *become* production (updating `DATABASE_URL`/`DATABASE_MIGRATOR_URL` in Render to point at the restored branch, or an equivalent promotion step, then redeploying). That is the step a real incident would actually need, and it's materially more consequential than branch creation — reasonably left untested in a routine drill, but explicitly still open here rather than assumed to work.
- `PACK_2_11F` (Backup/Recovery Controls) still only certifies the application-level *metadata model and dry-run orchestration* — this drill exercised Neon's own platform mechanism directly, a different (and, as of this drill, more concretely verified) thing.

## When to use this

Two different failure shapes need different responses — don't reach for a full rollback when a targeted fix would do:

- **Application-level failure** (a bad deploy, a bug that breaks a workflow, but the database is intact and correct) → §1 below, application rollback only.
- **Data-level failure** (corrupted, deleted, or badly-migrated data; a botched manual database change) → §2 below, database recovery. Its core mechanism (getting a correct historical copy) is now verified; cutting production over to it is not.

## 1. Application-level rollback (revert a bad deploy)

This part IS mechanically straightforward given how this system is deployed — no new capability needs to be built, just executed carefully:

1. Identify the last known-good commit on `smp1/production-parity` (check `git log`, cross-reference against the last confirmed-working acceptance evidence, e.g. `Phase-3-Fresh-Authenticated-ERP-Acceptance-2026-09-16.md`'s commit `14bf97e` was confirmed working as of that date).
2. In the Render dashboard, trigger a manual deploy of that earlier commit for both `gifthatkeos-standalone-v1-api` and `gifthatkeos-standalone-v1-web` — `autoDeployTrigger` is off, so this is always a deliberate, manual action, which is a safety feature here, not friction.
3. **Do not roll back database migrations to match an older application version unless the newer migrations are confirmed to be the actual cause of the problem.** Migrations in this codebase are additive (new tables/columns), so an older application version will generally still function against a newer schema — rolling the schema back too is usually unnecessary and adds risk.
4. Re-run the Phase 3-style read-only acceptance pass (or at minimum, load the Dashboard and the specific workspace that was broken) to confirm the rollback actually fixed the problem before declaring it resolved.

## 2. Database-level recovery (data corruption/loss) — mechanics verified 2026-09-21; the 6-hour window is the real constraint

1. **Stop writes immediately** — this likely means taking the API service down (Render dashboard, suspend/stop the service) rather than trying to selectively block writes at the application layer under pressure.
2. **Check the clock first.** If the failure happened more than 6 hours ago, Neon's own point-in-time recovery cannot reach it — there is no snapshot backstop today (see above). At that point this procedure has nothing further to offer; recovery would mean manual re-entry of lost data, not a database-level restore.
3. If within the 6-hour window: in the Neon console, **Backup & Restore → Restore from history**, pick the timestamp just before the failure, and create a new branch from it (confirmed 2026-09-21: this takes seconds and produces a correct, queryable copy). Use the SQL Editor or a direct connection to that branch to confirm it looks right before going further.
4. **Not yet tested — this is the actual gap**: making that branch become production. The concrete mechanism is updating Render's `DATABASE_URL` and `DATABASE_MIGRATOR_URL` environment variables to the restored branch's connection strings, then redeploying the API service — but this exact swap has not been rehearsed. Treat it as the next thing to verify, ideally before it's needed for real.
5. Everything written between the restore point and the failure is lost unless it can be manually re-entered. Decide whether that's acceptable before proceeding — this is often the actual hard decision in a real incident, not the technical mechanics.
6. After any database-level recovery, re-run the migration check (`npm run migrate:latest --workspace=@gifthatkeos/database` against `DATABASE_MIGRATOR_URL`) to confirm the restored database is still at the expected migration state, then the full Phase 3-style acceptance pass again.

## 3. Decision authority

**Confirmed, 2026-09-21: Hitendra Chug.** As the sole operator (see `SMP1-Support-Escalation-Ownership-DRAFT-2026-09-21.md`), decision authority for a destructive database-level restore — including the intervening-data-loss call in §2.5 — rests with him. No delegation or secondary approver exists.

## 4. The 6-hour recovery window — accepted, 2026-09-21

**Operator decision: the 6-hour Neon PITR window is acceptable as-is.** No change to the Neon plan or configuration of the (currently unused) snapshot feature was requested. This means, explicitly: if a data-level failure isn't caught and acted on within 6 hours of it happening, there is no database-level recovery path — only manual re-entry of whatever was lost. That's now a known, accepted risk, not an oversight.

## Recommended before this procedure is trusted for real

1. ~~Run an actual restore drill against a Neon branch~~ — **done, 2026-09-21.** See above.
2. ~~Confirm whether the current Neon plan supports point-in-time recovery/branching, and the actual retention window~~ — **done, 2026-09-21: Free plan, 6 hours, boundary confirmed exactly.**
3. ~~Rehearse actually promoting a restored branch to production (§2.4)~~ — **done, 2026-09-22.** See execution notes below.
4. ~~Fill in §3's decision authority explicitly~~ — **done, 2026-09-21: Hitendra Chug.**
5. ~~Decide whether the 6-hour recovery window is acceptable~~ — **done, 2026-09-21: accepted as-is.** See §4.

**All items are now closed.** This procedure can be considered fully trustworthy as written, with one correction folded into §2.4 below.

## §2.4 execution notes, 2026-09-22 — the production-cutover rehearsal, performed for real

Executed end-to-end against live production, with the operator (Hitendra Chug) at the controls for every step touching secrets, per §3's decision authority and because Render masks environment variable values — there was no way to read and safely restore the original `DATABASE_URL`/`DATABASE_MIGRATOR_URL` without the operator's own hands on that specific step.

**Sequence actually run:**
1. Operator suspended `gifthatkeos-standalone-v1-api` in the Render dashboard (confirmed via `erp.gifthatke.in/health` returning "Service Suspended").
2. A Neon branch (`cutover-drill-2026-09-22`, from `production`, "Branch data and schema" / current moment) was created only after the suspend, so it's a bit-for-bit copy with zero gap — nothing written between suspend and branch creation, because nothing could be.
3. Operator updated `DATABASE_URL` (pooled) and `DATABASE_MIGRATOR_URL` (direct) to the branch's connection strings, after first saving the original values, then resumed the service.
4. **First real finding**: after Resume, Neon showed no new activity on the drill branch — `production`'s compute stayed the one showing recent activity. Traced via the Events log: Suspend and Resume are the only two events recorded; no Deploy event fired in between. **Resuming a suspended Render service does not perform a fresh deploy and does not reliably pick up environment variable changes make while it was suspended — it restarts the previous running configuration.** A real incident response that stops here, believing the swap took effect, would keep serving from the old (potentially failed/corrupted) database while believing it had cut over.
5. Fix: triggered an explicit Manual Deploy → Deploy latest commit (same commit, forced rebuild + fresh container). This time it worked.
6. Verified genuinely — not just inferred — via Neon's per-branch compute monitoring graph (Postgres → Monitoring → CPU), which shows real `Used` CPU at precise timestamps. A `/health/database` request against `erp.gifthatke.in` was immediately followed by checking Neon's graph at that same timestamp: the drill branch showed live CPU usage (`0.04` vCPUs) at the exact second of the request; `production`'s own graph showed no corresponding new activity in that window. This is a materially stronger check than the branch list's summary "Compute last active" column, which turned out to be a stale/lagging billing-period rollup, not a live indicator — it continued to say `production... now` throughout, even while `production` was demonstrably not being queried. **Don't trust that column for this kind of verification; use the per-branch Monitoring → CPU graph with a timestamp-correlated request instead.**
7. Reversed cleanly: operator restored the original `DATABASE_URL`/`DATABASE_MIGRATOR_URL` values, another explicit Manual Deploy was triggered (not Resume, having learned the lesson from step 4), and the same timestamp-correlated verification confirmed `production` active again and the drill branch back to idle.
8. Drill branch deleted. No residue: back to 1 branch, matching the same clean-teardown standard the 2026-09-21 drill set.

**Net result: the mechanism genuinely works, cutover and cutover-back both, with zero data loss** (writes were stopped before the branch was taken, and the original production branch was never written to or otherwise touched at any point during the drill). Total production outage: approximately 24 minutes (9:30 PM suspend to 9:53 PM final live-and-verified redeploy) — longer than a well-drilled real incident should take, entirely because of the Resume-doesn't-redeploy discovery mid-drill; a second attempt, now that this is known, would be faster.

**Correction to step 4 of §2 above**: "updating Render's `DATABASE_URL` and `DATABASE_MIGRATOR_URL` environment variables... then redeploying" was already the right instruction — the gotcha this rehearsal found is specifically that *resuming a suspended service is not the same as redeploying it*, and anyone following §2 in a real incident must trigger an explicit deploy (Manual Deploy → Deploy latest commit, or equivalent), not just bring the service back up, or the cutover silently doesn't happen.

GAP-001's Domain 38 finding (production-cutover-rehearsal gap) is now closed. This procedure has never been more thoroughly verified than it is as of this update.
