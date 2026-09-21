# SMP1 Standalone — Cutover Sequence

**Date:** 2026-09-21 (all open questions resolved same day)
**Status:** All scenario/scope questions the original draft raised have been answered directly by the operator. The remaining content is now the operative go-live plan, not an unresolved proposal — the one thing not yet done is executing "What's actually needed now" below (creating employee accounts).
**Origin:** `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §4, one of six categories that pass identified as unresearched.

## An open question this draft can't answer on its own

The operator confirmed (2026-09-21) that the frozen Apps Script tool is **not** currently in live use — Standalone is the only live system. What isn't clear from that alone is **which of these three situations is actually true**, and this draft needs to be read differently depending on which one it is:

- **(a)** Cutover already happened in practice (every employee already works in Standalone day to day), just never with a formal documented process — in which case this document should be used as a **retroactive verification checklist**, confirming nothing was missed.
- **(b)** Frozen was retired from use before Standalone was fully adopted, and some employees or workflows aren't fully on Standalone yet — in which case this is a **forward plan** for finishing the switch.
- **(c)** Frozen was never in real day-to-day production use to begin with (e.g., this was built before full operational rollout) — in which case "cutover" is really "first go-live," not a switch from one system to another.

**Resolved, 2026-09-21: scenario (c).** The operator confirmed frozen's data was never real production data, and the explicit goal is to hand Standalone over completely new to employees, live and ready to use as soon as possible. This is a **first go-live, not a switch from an established system** — there is no real historical continuity to protect, no meaningful "cutover" moment where employees stop using one system and start another, and (per the operator's separate decision, recorded in "What this draft does not cover" below) no historical data migration. Everything below is rewritten with that in mind: the priority is getting employees actually able to sign in and use the system, not managing a transition away from something that was never really in use.

## Go-live readiness gate

Before employees are told to start using Standalone:

- [x] Production deployment current — confirmed 2026-09-21: `smp1/production-parity` at commit `15db3e9`, both Render services (`gifthatkeos-standalone-v1-api`/`-web`) live, Neon Postgres, live at `https://erp.gifthatke.in`.
- [x] Database migrations applied — confirmed current.
- [x] Read-only acceptance pass across every employee workspace — confirmed 2026-09-16 (`Phase-3-Fresh-Authenticated-ERP-Acceptance-2026-09-16.md`), all 12 workspaces load and function.
- [x] Backup/restore mechanism verified — confirmed 2026-09-21 (drill against a disposable Neon branch); 6-hour recovery window explicitly accepted by the operator as sufficient for now.
- [x] GAP-002 (secret custody) — closed 2026-09-21, Render's own environment-variable storage accepted as-is.
- [ ] **The one real open item: employee accounts.** Only one confirmed user exists in the system today (Hitendra Chug, Super Administrator). Every other employee who needs to use Standalone needs an account created and a role assigned before they can sign in — see "What's actually needed now" below.
- GAP-009's operational half (monitoring beyond Render's basic dashboard) and GAP-011 (operator attestation) remain open but don't block day-to-day employee use — tracked separately, not a go-live blocker.

## What's actually needed now

Given this is a first go-live, not a migration away from a real system, the sequence is much shorter than a traditional cutover:

1. **Create accounts for every employee who needs access**, via the Users workspace (Settings → Users, or wherever the live UI currently exposes it), assigning each the correct role. This is the one concrete remaining step between "system is ready" and "employees can actually use it."
2. **Give each employee the URL** (`https://erp.gifthatke.in`) and confirm they can sign in with their Google account.
3. **No freeze/decommission step is needed** — frozen was never carrying real production data, so there's nothing to protect a transition away from. It stays exactly as TITAN LOCK already requires (permanently preserved, read-only, never mutated) with no special go-live-related action needed on it.
4. **Record the go-live date** here once employees actually start using it, so "when did this happen" isn't lost to memory.

## What this draft does not cover

- Data migration between frozen and Standalone. **Operator decision, 2026-09-21 (superseding an earlier same-day answer): no — frozen's data was never real production data, so there's nothing worth migrating.** This closes the "data and business reconciliation" question that Phase 4's evidence pack had flagged as genuinely open — it's now resolved as "not applicable," not left outstanding.
- Training/change-management for employees — not something this document can propose without knowing the team, beyond the bare account-creation step above.

**All prior open questions in this draft are now resolved**: scenario is (c); no historical data migration; single-operator-scale confirmed elsewhere (`SMP1-Support-Escalation-Ownership-DRAFT-2026-09-21.md`). The one remaining action is employee account creation, above — not a correction to this document, but the actual next step.
