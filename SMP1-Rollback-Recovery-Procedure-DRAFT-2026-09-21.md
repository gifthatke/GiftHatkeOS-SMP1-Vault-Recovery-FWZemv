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

**Not specified — needs the operator to confirm.** Given the small operator footprint observed in this system, this is likely just "whoever has Render/Neon dashboard access decides," but that should be stated explicitly rather than assumed, especially for the destructive step in §2.2 (restoring to an earlier point loses intervening data).

## Recommended before this procedure is trusted for real

1. ~~Run an actual restore drill against a Neon branch~~ — **done, 2026-09-21.** See above.
2. ~~Confirm whether the current Neon plan supports point-in-time recovery/branching, and the actual retention window~~ — **done, 2026-09-21: Free plan, 6 hours, boundary confirmed exactly.**
3. **Still open**: rehearse actually promoting a restored branch to production (§2.4) — the one part of this procedure that remains genuinely untested.
4. **Still open**: fill in §3's decision authority explicitly.
5. **New, from this drill**: decide whether the 6-hour recovery window is acceptable for this system as-is, or whether Neon's snapshot feature (currently unconfigured) should be set up to extend it. That's a real business/risk decision, not a technical one — this draft doesn't make it.

**Corrections needed from the operator**: confirm §3 (decision authority); decide on the 6-hour-window risk-acceptance question above; and ideally rehearse the still-untested production cutover step (§2.4) before relying on this procedure for real.
