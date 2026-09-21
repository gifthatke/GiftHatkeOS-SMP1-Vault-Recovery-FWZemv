# SMP1 Standalone — Cutover Sequence

**Date:** 2026-09-21
**Status:** DRAFT PROPOSAL — NOT AUTHORIZED, NOT CERTIFIED. Written to close a genuine gap in Phase 4's evidence pack (no cutover sequence existed anywhere in the corpus). This is a starting point for the operator to correct, not a governance record to be relied on as-is.
**Origin:** `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §4, one of six categories that pass identified as unresearched.

## An open question this draft can't answer on its own

The operator confirmed (2026-09-21) that the frozen Apps Script tool is **not** currently in live use — Standalone is the only live system. What isn't clear from that alone is **which of these three situations is actually true**, and this draft needs to be read differently depending on which one it is:

- **(a)** Cutover already happened in practice (every employee already works in Standalone day to day), just never with a formal documented process — in which case this document should be used as a **retroactive verification checklist**, confirming nothing was missed.
- **(b)** Frozen was retired from use before Standalone was fully adopted, and some employees or workflows aren't fully on Standalone yet — in which case this is a **forward plan** for finishing the switch.
- **(c)** Frozen was never in real day-to-day production use to begin with (e.g., this was built before full operational rollout) — in which case "cutover" is really "first go-live," not a switch from one system to another.

**Confirm which of these applies before treating anything below as settled** — the sequence is written to be usable in all three cases, but the framing changes what "done" means.

## Pre-cutover readiness gate

Before any employee is told to rely on Standalone (retroactively: before confirming this gate was actually satisfied):

- [ ] Production deployment current — confirmed 2026-09-16: `smp1/production-parity` at commit `14bf97e`, deployed to Render (`gifthatkeos-standalone-v1-api`/`-web`), Neon Postgres, live at `https://erp.gifthatke.in`.
- [ ] Database migrations applied — confirmed 2026-09-16: all migrations current, including the five from this session's own implementation wave.
- [ ] Read-only acceptance pass across every employee workspace — confirmed 2026-09-16: all 12 workspaces (`Phase-3-Fresh-Authenticated-ERP-Acceptance-2026-09-16.md`) load and function with real data.
- [ ] Every employee who needs access has a working, authorized Google account for sign-in, and knows the URL (`https://erp.gifthatke.in`).
- [ ] **Not yet satisfied**: a tested, verified backup/restore procedure exists for the production database. `PACK_2_11F` certifies the backup/recovery *metadata model* as closed, but explicitly leaves live backup execution, verified restore, RPO, and RTO as open. **Recommend testing this before treating cutover as irreversible in practice**, even if the formal switch has already happened socially.
- [ ] GAP-002 (secret custody) and GAP-011 (operator attestation) remain fully open. GAP-009 (topology/operational-ownership certificate) is now half-closed: its documentation half closed 2026-09-21 (`smp1-standalone-v1-render-neon-production-topology-2026-09-21.md`, Standalone repo); its operational half — live backup/restore, monitoring — remains open, same gap as the backup/restore point immediately above. None of these three block day-to-day employee use, but all should be tracked to closure independent of cutover itself.

## Sequence

Given the small operator footprint observed directly in this system (one confirmed Super Administrator, `support.gifthatke@gmail.com`/Hitendra Chug, one active order as of 2026-09-16) — a phased, module-by-module or employee-by-employee rollout is likely unnecessary overhead. **Proposed default: a single-step cutover**, not a phased one, unless the operator knows of a larger employee base this document isn't accounting for.

1. **Confirm the readiness gate above.**
2. **Freeze new data entry in frozen Apps Script**, if it isn't already — stop anyone from creating new orders/records there, even if it's still reachable for reference.
3. **Announce the switch** — every employee who used the frozen tool is told, as of a specific date/time, to use `https://erp.gifthatke.in` exclusively for new work.
4. **Leave frozen reachable but inert** for a reference/lookback period (proposed: 30 days, adjust to what's actually useful) — readable for historical lookup, not written to. This matches the existing TITAN LOCK principle that frozen remains a read-only reference and must not be mutated, just extends it from "the code" to "the live deployed tool" too, if one still exists.
5. **After the reference period, formally decommission** whatever's left of the live frozen deployment (revoke any remaining write triggers/webhooks, but do not delete the underlying Sheets/Script project — TITAN LOCK's frozen-reference preservation still applies to the artifact itself).
6. **Record the cutover date** in this document or a successor, so "when did this actually happen" isn't lost to memory.

## What this draft does not cover

- Data migration/reconciliation between frozen and Standalone (a separate, still-open Phase 4 category — `data and business reconciliation`). If any historical frozen data needs to exist in Standalone and doesn't yet, that's a distinct piece of work this sequence assumes is either already done or not needed.
- Training/change-management for employees — not something this document can propose without knowing the team.

**Corrections needed from the operator**: which of scenarios (a)/(b)/(c) above actually applies; whether the single-step rollout assumption is right or there's a larger team to account for; the actual cutover date if one already happened; and the reference-period length in step 4.
