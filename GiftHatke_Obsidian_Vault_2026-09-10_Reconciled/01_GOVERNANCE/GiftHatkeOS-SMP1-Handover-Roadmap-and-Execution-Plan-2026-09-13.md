# GiftHatkeOS SMP1 Handover Roadmap and Execution Plan

**Date:** 2026-09-13  
**Status:** PLANNING ARTIFACT — NOT AN IMPLEMENTATION OR DEPLOYMENT AUTHORIZATION  
**Programme:** Standalone Migration Programme 1 (SMP1)  
**Target:** Standalone v1.0 handover only

## 1. Current decision

The exact certified 44-domain Enterprise Canon has been recovered, checksummed,
reconciled into Obsidian, and published to the active repository. Canon Recovery
is therefore complete as a recovery/publication wave.

Exhaustive implementation parity and handover are not yet provable. The next
work is a controlled evidence and remediation sequence. No work in this plan
creates a new business capability or changes the certified architecture.

## 2. Immutable baselines

| Item | Authoritative value |
|---|---|
| Active repository | `gifthatke/GiftHatkeOS-Standalone` |
| Branch | `smp1/production-parity` |
| Verified branch head | `be75043bc4616114cad541fbc64583dff0333c3b` |
| Canon publication commit | `dce36d31ce44350817b238fbfdc63df68c6e65c6` |
| Canon closure commit | `be75043bc4616114cad541fbc64583dff0333c3b` |
| Frozen repository | `gifthatke/GiftHatkeOS` |
| Required frozen HEAD | `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87` |
| Raw Canon source SHA-256 | `30d77719c00a710337c91e8f2fdf9d98ac98fb9a81cb9aaddfc4085ca75479f4` |
| Recovered registry SHA-256 | `dc996236782651c8994ff990a8b634fc1dbffa9ed6bb0b167ac5edd94a8d65ec` |
| Canon source | Shared conversation “Order Management Canon Stage 3” |
| Live ERP | `https://erp.gifthatke.in` |
| Obsidian vault | `GiftHatke_Obsidian_Vault_2026-09-10_Reconciled` |
| Automatic deployment | Disabled |

The branch has been verified identical to the closure commit. The frozen
repository remains a read-only reference and must not be mutated.

## 3. Titan Lock controls

These controls apply to every phase:

- Do not modify the frozen Apps Script reference or frozen repository.
- Do not redesign, infer, rename, merge, split, or extend the 44-domain Canon.
- Do not create Domain 45, permission #110, reseller/retailer/partner dashboards,
  ERP9, ERP10, or Version 1.1 work.
- Keep User Management read-only. Do not add employee creation, role or
  permission administration, assignment, invitation, or onboarding UI.
- Keep Settings certified/live/closed at 100% for its current wave. Do not
  choose, enter, reveal, move, or persist a production secret or secret provider
  in the Canon wave.
- Preserve all closed modules: Dashboard, Orders, Production, Inventory, CRM,
  Customers, Personalization, Procurement, Shipping, Finance, Reports, User
  Management, and Settings.
- No application code, dependencies, migrations, routes, permissions, APIs,
  UI, deployment, database, or business-behavior change is authorized by this
  planning document.
- Any implementation or operational change requires its own exact Titan Lock,
  scope record, tests, acceptance, and additive commit.
- Git history is immutable: no reset, amend, rebase, force-push, or tag mutation.

## 4. Current evidence and blockers

The published registry has exactly 44 sequential rows and no Domain 45. The
published parity matrix has 44 rows: 43 are classified **partial foundation**
and Domain 42 is **post-SMP1 / ERP9 / ERP10 / Version 1.1**. No domain is
promoted to complete by publication alone.

The following findings remain open and blocking:

| Finding | Classification | Required path |
|---|---|---|
| `SMP1-GAP-001` | missing and requiring a new Titan Lock wave | Requirement-level Canon-to-frozen-to-Standalone reconciliation and fresh authenticated live acceptance |
| `SMP1-GAP-002` | missing and requiring a new Titan Lock wave | Separate secret-custody/security wave; provider selection is not part of this plan |
| `SMP1-GAP-009` | missing and requiring a new Titan Lock wave | Render/Neon topology, backup, restore, rollback, and operational-ownership certificate |
| `SMP1-GAP-011` | partial foundation | Operator-side clean-worktree and frozen-HEAD attestation |

**Correction, 2026-09-16**: `SMP1-GAP-005` and `SMP1-GAP-006` were removed from
this table — both were actually closed on 2026-09-13 itself
(`smp1-gap-005-reports-regression-closure-2026-09-13.md`,
`smp1-gap-006-dependency-deployment-closure-2026-09-13.md`), the same day
this table was written; the closures simply were never reflected back into
this table or into the finding-register documents, which still show them
OPEN/BLOCKING to this day. Found and corrected during the Phase 4
reconciliation pass — full detail, plus a similarly-stale `SMP1-GAP-008`
entry (Reports Operations & Risk provider, apparently resolved by this
session's PHB-7 work but never closed on paper) and GAP-001's own silent
identity shift, in `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md`
(vault root). No finding classification was changed without the evidence
cited there.

Non-blocking findings remain open under their existing classifications. No
finding may be removed, merged, downgraded, or marked resolved without direct
authority evidence.

## 5. Roadmap and execution gates

### Phase 0 — Baseline control check (complete)

Confirm repository, branch, verified head, remote alignment, frozen HEAD,
automatic-deployment state, retained Canon records, and Obsidian location.

**Exit:** all immutable baselines recorded; no frozen mutation.

### Phase 1 — Canon recovery and publication (complete)

Preserve the exact source, provenance, raw-source checksum, registry checksum,
numbering, names, ordering, boundaries, and certification states. Publish the
registry, 44-row matrix, final reconciliation, finding register, provenance, and
closure records additively.

**Exit:** commits `dce36d3…` and `be75043…` verified on the remote branch;
Obsidian reconciliation package retained.

### Phase 2 — Static 44-domain parity reconciliation (evidence-bound)

For each domain, trace the Canon requirement against the frozen Apps Script
reference, repository implementation, persistence, ports/adapters,
services/routes, permissions/authentication, workspace/UI, deployment topology,
tests, and governance evidence. Treat path presence as a lead, never as proof
of behavioral equivalence.

**Exit:** every row has an evidence card, an exact classification, provenance,
and an explicit unresolved boundary. This phase does not authorize missing
functionality.

### Phase 3 — Fresh authenticated ERP acceptance (complete, 2026-09-16)

Open the authenticated cloud browser first. Verify the current employee ERP
principal, reachability, workspace loading, and application-origin errors. Run
only safe read-only checks across the certified employee workspaces and record
the exact URL/state, timestamp, and observed result. Do not submit forms, mutate
records, enter secrets, or change settings.

**Exit:** fresh acceptance evidence exists for every tested surface, or the
limitation is recorded without claiming acceptance. If the cloud browser is
quota-blocked, stop that attempt and preserve the limitation; do not bypass it.

**Status update, 2026-09-16:** the prior cloud-browser quota block no longer
applied — this pass used an authenticated, operator-driven Chrome session
(Claude in Chrome) rather than a separate cloud-browser quota, with no
credentials entered by Claude at any point. All 12 certified employee
workspaces (Dashboard, Orders, Production, Inventory, CRM, Customers,
Personalization, Shipping, Finance, Reports, Users, Settings) were reached,
loaded without application-origin errors, and rendered real production data —
strictly read-only throughout, no forms submitted, no records mutated, no
settings changed. This also served as first live confirmation of this
session's own implementation wave (GAP-001's consolidated pre-handover
plan, PHB-1 through PHB-7 and Tier-2 items 7-8), deployed and migrated
immediately beforehand: the Reports workspace's Executive Intelligence Hub
showed all 8 modules Connected at 100% health with real computed figures,
and Finance's seed data (3 planning scenarios, 8 cost centers) matched
exactly. Two non-blocking findings recorded, not fixed: the Dashboard's own
"Employee module connectivity" widget mislabels Shipping and Finance as
"Wave 2" despite both functioning correctly, and Finance's frontend reads
as a rawer data-inventory view next to the other workspaces' polish. Full
per-surface evidence: `Phase-3-Fresh-Authenticated-ERP-Acceptance-
2026-09-16.md`.

### Phase 4 — Handover-readiness evidence pack (partial, 2026-09-16 — see status update)

Reconcile the Canon matrix, finding register, migration mapping, data and
business reconciliation, cutover sequence, rollback/recovery procedure,
backup/restore evidence, authoritative-writer control, permission validation,
regression evidence, support/escalation ownership, monitoring, and the location
of current operating documentation.

**Exit:** the operating team can locate and use the current standalone guidance;
legacy Apps Script material remains reference-only and is not primary standalone
operating guidance.

**Status update, 2026-09-16:** four parallel research passes reconciled the
Canon matrix, finding register, and domain-certification currency across the
full `docs/governance/` (116 files) and `docs/certification/` (49 files)
trees against current production reality. Concrete corrections applied
directly to this document (§4's blocker table above) and cataloged in full
in `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` (vault root),
including: nine stale DigitalOcean-era topology documents (production is
actually Render + Neon, already self-flagged by the corpus as GAP-009 but
never superseded), a dead `.do/app.yaml` still in the repo, a release tag
380 commits behind current HEAD, two finding-register entries corrected
(§4 above), and eleven newly-built capabilities from this session's own
GAP-001 implementation wave (PHB-1 through PHB-7, items 7-8) with zero
certification/closure record anywhere in the corpus — three of which
(Shipping Intelligence, Order Notes, Order Attachments) were previously
recorded as deliberate deferrals that this work now fulfills but never
closed out on paper.

**Six of Phase 4's twelve categories were NOT covered by this pass and
remain open**: migration mapping, data and business reconciliation, cutover
sequence, rollback/recovery procedure, authoritative-writer control, support/
escalation ownership, and monitoring (permission validation and regression
evidence are separately covered by this session's own test-suite results;
backup/restore evidence is covered — see the genuinely-open findings in the
evidence pack). This phase is not complete; the evidence pack document is
explicit about exactly what remains.

**Status update, 2026-09-21:** four of those six categories closed. The
operator directly confirmed the frozen Apps Script tool is not in live use —
Standalone is the sole live system — closing **authoritative-writer control**
on real evidence rather than the "unverified" status the corpus had
self-flagged. **Cutover sequence**, **rollback/recovery procedure**, and
**support/escalation ownership** each gained a DRAFT proposal (not
authorized/certified, explicit open questions for the operator to resolve):
`SMP1-Cutover-Sequence-DRAFT-2026-09-21.md`,
`SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md`,
`SMP1-Support-Escalation-Ownership-DRAFT-2026-09-21.md` (all vault root).
**Migration mapping, data/business reconciliation, and monitoring remain
genuinely open** — `docs/canon-mapping/` was checked directly and found to
contain only a one-paragraph methodology statement, no actual crosswalk.
Full detail: `Phase-4-Handover-Readiness-Evidence-Pack-2026-09-16.md` §4
(updated 2026-09-21).

### Phase 5 — Separate authorized remediation waves (only if still required)

Execute one lock at a time, in this order unless evidence changes the critical
path:

1. **Topology and recovery certificate (`GAP-009`)** — certify the actual
   Render/Neon topology, database ownership, backup/restore, rollback, DNS,
   OAuth origin, cold-start/SLA posture, and operational ownership. No platform
   redesign.
2. **Web regression gate (`GAP-005`)** — correct the known gate issue and wire
   the root/web test orchestration. Run the complete regression suite and record
   results.
3. **Dependency security (`GAP-006`)** — scope the smallest approved dependency
   patch, rerun build/typecheck/tests, and repeat deployment and authenticated
   acceptance only under that lock.
4. **Production secret custody (`GAP-002`)** — select and certify a provider,
   encryption/key lifecycle, adapter, deployment configuration, backup/recovery,
   fail-closed behavior, and live acceptance. Never place secret values in Git,
   Obsidian, logs, or chat.
5. **Operator attestation (`GAP-011`)** — record clean status for the active
   worktree and exact frozen HEAD; do not restore or delete unrelated user files
   automatically.

Each remediation wave requires its own scope lock, implementation evidence,
tests, deployment/rollback evidence where applicable, Obsidian update, and
ordinary non-force additive commit. A wave may not silently reopen a closed
module.

### Phase 6 — Final handover decision gate

Prepare handover only when all of the following are directly evidenced or
formally accepted by the correct authority:

- exact 44-domain Canon and provenance are preserved;
- every Canon row has a requirement-level disposition;
- frozen Apps Script/repository baseline is unchanged;
- active branch, remote, worktree, and deployment records reconcile;
- regression, permission/authentication, backup/restore, rollback, and recovery
  evidence pass;
- authoritative-writer and cutover controls are proven;
- all blocking findings are resolved or explicitly accepted without silent
  downgrade;
- fresh authenticated live ERP acceptance passes;
- support, escalation, monitoring, ownership, and current documentation are
  operationally usable;
- User Management remains read-only and Settings remains closed as decided;
- no future-domain or ERP9/ERP10/Version 1.1 work has entered SMP1.

If any gate fails, handover remains **NOT AUTHORIZED** and the finding register
stays open.

## 6. Execution protocol for every wave

1. Record the exact authority, scope, baseline head, and exclusions.
2. Perform read-only discovery before any permitted change.
3. Validate exact scope before staging, committing, or pushing.
4. Run targeted tests, then the complete relevant regression gate.
5. Capture source, runtime, deployment, acceptance, and governance evidence.
6. Reconcile Obsidian after the milestone and before closure.
7. Make one additive commit with no unrelated files or deletions.
8. Verify local/remote/frozen heads and worktree status without force operations.
9. Update the finding register without removing or silently changing findings.
10. Report the wave percentage and the four programme percentages.

## 7. Timing and critical path

These are planning ranges, not promises:

- Once cloud-browser access is restored, Phase 3 can be completed in one focused
  evidence session if the ERP remains reachable and no new failure appears.
- The operator attestation is a short independent gate.
- The topology, regression, dependency, and secret-custody waves are separate
  controlled activities. Best-case closure is several focused sessions; a
  realistic planning range is **3–7 working days** after required access and
  decisions are available.
- If provider selection, backup/restore testing, or deployment ownership needs
  new setup, **1–3 weeks** is more realistic.
- The largest uncertainty is `GAP-002` secret custody and its recovery/live-
  acceptance proof. No handover date is valid until that gate is evidenced.

## 8. Immediate next action

The next authorized action is **Phase 3: restored authenticated, read-only ERP
acceptance plus requirement-level Canon parity checks**. The previous attempt
was blocked by the cloud-browser usage limit; that limitation must be recorded,
not bypassed. No implementation wave begins until the evidence gate and current
findings are reconciled.

**Current completion:** Canon Recovery **100%** · User Management **100%** ·
Settings **100%** · Overall SMP1 **less than 100% / handover blocked**.
