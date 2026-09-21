# SMP1 Standalone — Support and Escalation Ownership

**Date:** 2026-09-21
**Status:** DRAFT PROPOSAL — NOT AUTHORIZED, NOT CERTIFIED. This is fundamentally an organizational decision, not a technical one — this draft exists so there's something concrete to correct rather than a blank page, not because it can be determined from the codebase or governance corpus.
**Origin:** `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §4.

## What's actually known vs. assumed

Known directly from the system itself (Users workspace, 2026-09-16): there is exactly one confirmed user, **Hitendra Chug (`support.gifthatke@gmail.com`), role Super Administrator**. Whether there are other employees who use this system but don't yet have accounts, or whether this genuinely is a one-operator business at this stage, is not something the system itself can answer — **this draft assumes a small operation and should be told otherwise if that's wrong.**

## Proposed tiers (adjust freely — this is a starting shape, not a fixed structure)

**Tier 0 — self-service.** For anything an employee can resolve by re-reading the workspace, retrying, or checking whether it's a real data issue vs. a misunderstanding of the workflow. No formal ownership needed here beyond whatever onboarding/training material exists (none currently found in this corpus — a gap worth knowing about if the team grows).

**Tier 1 — operator-level issues.** Things the Super Administrator can fix directly: permission grants, data corrections through the UI, Settings changes, re-running a stuck workflow. Proposed owner: Hitendra Chug, as the only confirmed administrator today.

**Tier 2 — issues requiring code, deployment, or database changes.** Anything Tier 1 can't fix without touching the actual system — a bug, a deploy, a migration, a data-integrity problem beyond what the UI can correct. **This is the biggest open question**: is there an ongoing relationship with whoever built/maintains this system (this Claude Code session's own work, or a human developer/agency), or does Tier 2 currently have no defined owner at all? If the latter, that's a real handover risk worth surfacing plainly rather than leaving implicit.

**Tier 3 — infrastructure/platform issues.** Render or Neon outages, DNS/domain issues (`erp.gifthatke.in`), Google OAuth configuration problems. Owner would be whoever holds the Render/Neon/domain-registrar/Google Cloud Console credentials — likely the same person as Tier 1 today, but worth stating explicitly since these are different skill sets and different account access than day-to-day operation. Concrete service identifiers this tier would need are now on record in `smp1-standalone-v1-render-neon-production-topology-2026-09-21.md` (Standalone repo) — API service ID, region, migration-seam variable name — rather than needing to be rediscovered during an actual incident.

## Known current limitations Tier 1/2 support should be aware of

Carried over directly from this session's own findings, so whoever's supporting this system isn't caught off guard by something already known:

- `GET /health` returns a hardcoded constant, not a real system-health probe — don't trust it as a signal that the database is actually reachable.
- The Dashboard's "Employee module connectivity" widget mislabels Shipping and Finance as "Wave 2" even though both work correctly — a cosmetic issue, not a real outage indicator, but worth knowing so it isn't chased as a bug.
- Finance's frontend is a rawer, more basic view than the other 11 employee workspaces — the backend capability (receipts, expenses, budgets, forecasts) is fully built and reachable, the UI just hasn't caught up. Not a bug, but will look unfinished to a new user.
- No live backup/restore has ever been tested (see `SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md`) — a real, not-yet-closed operational risk.
- Deploys are manual (`autoDeployTrigger: off`) — nothing on this system updates itself; someone has to actively trigger a Render deploy for any code change to take effect, and separately run the database migration command for any schema change.
- Whoever picks up Tier 2 (code/deployment) support should know: implementation records for everything built in this continuation session (Finance write workflows, Financial Planning, all six Business Intelligence modules, Order Notes/Attachments, Today's Work, the Executive Dashboard, and two transaction-safety fixes) live in one place — `GAP-001-Pre-Handover-Implementation-Plan-2026-09-15.md` — not scattered across per-capability certification documents. That consolidation was a deliberate 2026-09-21 decision (`smp1-per-capability-certification-retirement-2026-09-21.md`, Standalone repo), not an oversight.

## What this draft cannot decide

- Who Tier 2/3 actually are, if not the current operator.
- Response-time expectations for any tier.
- Whether a formal ticketing/issue-tracking system is wanted, or whether direct contact is sufficient for the current scale.
- Budget/cost for any paid support arrangement.

**Corrections needed from the operator**: confirm or correct the assumed one-operator scale; name a Tier 2 owner (or explicitly accept there isn't one yet); confirm Tier 3 credential ownership; decide whether any of the "what this draft cannot decide" items actually need deciding now versus later.
