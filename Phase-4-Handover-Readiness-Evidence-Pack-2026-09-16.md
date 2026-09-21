# Phase 4 — Handover-Readiness Evidence Pack

**Date:** 2026-09-16
**Authority:** operator-directed full reconciliation, following `GiftHatkeOS-SMP1-Handover-Roadmap-and-Execution-Plan-2026-09-13.md` §5 Phase 4: "Reconcile the Canon matrix, finding register, migration mapping, data and business reconciliation, cutover sequence, rollback/recovery procedure, backup/restore evidence, authoritative-writer control, permission validation, regression evidence, support/escalation ownership, monitoring, and the location of current operating documentation. Exit: the operating team can locate and use the current standalone guidance."
**Method:** four parallel research passes over the full `docs/governance/` (116 files) and `docs/certification/` (49 files) trees in `GiftHatkeOS-Standalone`, cross-referenced against the vault's own governance folder, this session's GAP-001 consolidated implementation plan, and the Phase 3 live acceptance record — plus direct verification of specific claims (git tags, commit ages) below. This is a genuinely large reconciliation; some of Phase 4's twelve categories were fully covered by this pass, others were not researched at all and are marked as such rather than guessed at.

**Status:** this document reconciles and corrects what the existing 165+ files claim. It does not itself close GAP-009, GAP-002, or GAP-011, does not perform a live backup/restore test, and does not decide any open business question — those remain separately gated exactly as the roadmap requires.

---

## 1. Corrections needed — stale or contradicted documents

These documents make claims that current reality (verified directly, not assumed) now contradicts. None of the underlying certified work is wrong for what it certified at the time; the gap is that nothing has updated them since.

### 1.1 Topology and deployment (all predate the actual Render/Neon deployment)

| File | Stale claim | Current reality |
|---|---|---|
| `docs/governance/smp1-standalone-v1-digitalocean-deployment-target-selection.md` | "HOSTING: DIGITALOCEAN APP PLATFORM... DATABASE: DIGITALOCEAN MANAGED POSTGRESQL"; "no configured `origin` remote" | Production is Render (`gifthatkeos-standalone-v1-api`/`-web`) + Neon Postgres, live at `https://erp.gifthatke.in`. `origin` is configured and pushed (`https://github.com/gifthatke/GiftHatkeOS-Standalone.git`). |
| `smp1-standalone-v1-production-release-topology-and-operational-ownership.md` | "No hosting vendor is selected... belongs to the next operational release gate" | Render was selected and has been live for weeks. |
| `smp1-standalone-v1-digitalocean-app-spec-contract.md` / `-certification.md` | Certifies `.do/app.yaml` as the deployment asset | `render.yaml` (last touched 2026-09-14) is the actual, current deployment spec. **`.do/app.yaml` is a dead vestigial file still sitting in the repo root** — worth deleting or clearly marking superseded so it doesn't mislead a future reader. |
| `smp1-standalone-v1-git-release-remote-and-branch-ownership.md`, `-github-account-ownership-correction.md`, `-first-remote-preservation-certification.md` | "No configured origin... remote repository not created" | Remote exists, target identity (`gifthatke/GiftHatkeOS-Standalone`, branch `smp1/production-parity`) matches what these docs assigned — only the "not yet created" status is stale. |
| `smp1-standalone-v1-go-live-closure-certification.md` | "Git push: NOT PERFORMED. Release tag: NOT CREATED. Production deployment: NOT PERFORMED." | All three have since happened, via Render rather than the DigitalOcean path this document anticipated. |
| `smp1-standalone-v1-immutable-release-tag-certification.md` | Certifies `v1.0.0-STANDALONE-CERTIFIED` as the release marker | Verified directly: this tag points to `4a5b6a1224ba63967f8a61fd335b8deb2675b614` (2026-08-21). Current production HEAD is `14bf97e35a241c3b93a273af666af77ddf8823ca` (2026-09-16) — **380 commits and 26 days ahead of the tag**. Anyone treating this tag as "what's in production" would be badly wrong. No newer release tag exists. |

**This gap is not undiscovered** — the document set's own most recent reconciliation work (`smp1-overall-production-parity-gap-analysis.md`, `smp1-final-finding-register.md`, both 2026-09-13) already identified it, correctly described the real Render/Neon topology with evidence (`render.yaml`, live dashboard state, `autoDeployTrigger: off`), and logged it as **SMP1-GAP-009**, open/blocking, pending "a new Titan Lock wave" that was never subsequently written.

### 1.2 Finding register — two entries silently wrong, one identity silently reused

| File(s) | Issue |
|---|---|
| `smp1-finding-register-reconciliation-2026-09-13.md` (byte-identical repo + vault copy) | Still shows **GAP-005 and GAP-006 as OPEN/BLOCKING** — but `smp1-gap-005-reports-regression-closure-2026-09-13.md` and `smp1-gap-006-dependency-deployment-closure-2026-09-13.md`, both dated the same day, declare both CLOSED/IMPLEMENTED/VALIDATED. **This predates this session entirely** — the register was simply never updated after those two closures landed. |
| `GiftHatkeOS-SMP1-Handover-Roadmap-and-Execution-Plan-2026-09-13.md`, §4 blocker table | Same staleness — still lists GAP-005/006 as blocking, uncorrected even by this session's own 2026-09-16 edit (which only touched the Phase 3 section, not §4). |
| GAP-008 (Reports Operations & Risk provider unavailable) | Register: "OPEN; accepted provider baseline remains 7 of 8 connected." PHB-7 (this session) fixed the hard-coded `operationsRisk: undefined` and built real computation; Phase 3 live acceptance directly confirmed "Executive Intelligence Hub shows all 8 modules Connected, 0 Disconnected, 100% Health... Critical risks: 0 — real computed values." **No closure record exists for this anywhere.** |
| GAP-001's identity | Originally meant "exact certified 44-domain registry unavailable" — closed 2026-09-13 when the registry was recovered and published. The *same ID* was then silently reused from 2026-09-14 onward (`GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md`) to mean something categorically different: deep domain-by-domain Canon-to-frozen-to-Standalone semantic reconciliation. **Nothing marks this handoff explicitly.** A reader moving from the finding register to the vault's GAP-001 documents would reasonably assume continuous identity, when really: sub-finding A (registry) closed 2026-09-13 → ID reused for sub-finding B (semantic parity), opened 2026-09-14, still open. |

**No cross-copy divergence problem for this family specifically** — every finding-register/parity-matrix file that exists in both `docs/governance/` and the vault's `Canon-Reconciliation-2026-09-13/` folder is byte-identical (verified by diff). The staleness is uniform across both copies, not a reconciliation-between-copies problem.

### 1.3 Domain certification currency, by area

| File | Stale claim | Correction |
|---|---|---|
| `smp1-production-inventory-consumption-implementation-certification.md` + companion `-ownership.md`/`-ownership-certification.md` | "Cross-material database atomicity is not newly invented by this Standalone v1.0 parity slice" — certifies the OLD non-atomic, per-line posting behavior as closed/correct | This is exactly the defect Tier-2 item 8 fixed this session. The consumption sequence now runs inside one transaction (`platformRuntime.inventoryProcurementTransactions`). The certification's atomicity claim is now false of the live code; everything else it certifies (BOM formula, waste %, shortage semantics, MOV identity, negative-stock blocking) remains accurate. |
| `smp1-44-domain-parity-matrix-2026-09-13.md`, Domain 6 (Finance & Accounting) row | "Finance closure preserved... the closed read-only workspace" | PHB-1 added four Finance write routes (receipts/expenses/pay/reverse); PHB-2 added budgets/forecasts. Finance's HTTP transport is no longer read-only — live and deployed. This is the only stale row in an otherwise-unaffected 44-row matrix. |

**Everything else in these three clusters (Procurement/Inventory/Production: 66 of 69 files; Settings: 20 files; User Management: 7 files; the pre-existing Shipping workspace certs: 6 files; CRM/Customers/Personalization: 6 files; Finance foundation accounts/transactions/persistence: 2 files) remains accurate and uncontradicted** — narrow, dated, scope-locked slices that this session's changes don't touch. Don't treat the corrections above as casting doubt on the rest of the corpus; they don't.

---

## 2. New capability built this session with zero certification/closure record

Confirmed by direct keyword search across the full governance and certification corpus — none of the following appear anywhere as a completed/certified capability:

| Capability | Prior state in governance |
|---|---|
| Item 8 — Inventory BOM consumption transaction fix | Contradicted by an existing (now-stale) cert, see §1.3 — no new record of its own |
| Item 7 — CRM lead-to-order duplicate-conversion transaction fix | No record found in any of the four reconciliation passes |
| Inventory Intelligence (PHB-7) | No record |
| Production Intelligence (PHB-7) | No record |
| Finance write workflows (PHB-1) | No record beyond the now-corrected stale matrix row |
| Financial Planning — budgets/forecasts/scenarios/cost centers (PHB-2) | No record |
| Shipping Intelligence (PHB-7) | **Was explicitly pre-flagged as a "deferred boundary"** in `smp1-shipping-parity-scope-lock.md`: *"Executive Shipping Intelligence remains deferred until its ownership is reconciled against the already-certified Dashboard boundary."* This session built it; the deferral was never closed out on paper. |
| Order Notes (PHB-4) | **Was explicitly pre-flagged as a controlled deferral** in the Order mutation-workspace closure cert: *"separate notes-mutation capability beyond the current Order notes field."* Same pattern — built, not closed out. |
| Order Attachments (PHB-3) | **Was explicitly pre-flagged as deferred** in the Order read-workspace closure cert, alongside Order Notes. Same pattern. |
| Today's Work / task board (PHB-5) | No record — zero mentions of "today's work"/"task board" as a capability anywhere in the corpus |
| Executive Dashboard, both phases (PHB-6) | No record. The only Dashboard-adjacent finding is the pre-existing, still-open GAP-007 (stale "Wave 2" mislabeling), independently reconfirmed live in Phase 3 acceptance. |

**Not independently re-verified in this pass**: Sales Intelligence, Customer & CRM Intelligence, and Operations & Risk Intelligence (the other three of PHB-7's six Business Intelligence modules) were not individually searched for by name in any of the four research passes — only Inventory/Production Intelligence (explicitly checked) and Shipping Intelligence (surfaced incidentally via its pre-existing deferral note) were confirmed. Treat their documentation status as unknown, not as confirmed-missing, until checked.

**Recommendation, not performed here**: each row above needs its own dated closure record before this documentation set can be called current — this reconciliation identifies the gap, it doesn't fill it (that's new-document authorship, a separate, larger piece of work).

---

## 3. Genuinely open — confirmed by the documents' own honest self-disclosure, not contradicted

These aren't documentation staleness — the certifications are accurate about what they don't cover, and what they say is still missing is still missing:

- **Live backup/restore verification** — never executed against production Neon. `PACK_2_11F` (Backup/Recovery Controls) closes honestly as "CLOSED WITH RETAINED OPERATIONAL CONTROLS," explicitly retaining as unimplemented: physical backup execution, backup-artifact storage, any object-storage provider, `pg_dump`/`pg_restore`, destructive restore, cryptographic integrity verification, RPO, RTO, retention. The 44-domain matrix's Domain 38 (Business Continuity/DR) independently says the same thing: "Backup metadata and dry-run validation are insufficient production evidence." Both agree with each other and with this session's own earlier findings.
- **Diagnostics HTTP exposure** — deliberately deferred, no certified permission exists to publish it (`PACK_2_9B5B`). Matches the already-known fact that diagnostics/backup-recovery application services exist in code but aren't reachable via HTTP.
- **GAP-002** (secret custody) — untouched this session, business/security decision pending provider selection.
- **GAP-009** (topology certificate) — self-flagged by the documents themselves, needs the superseding Render/Neon record described in §1.1.
- **GAP-011** (operator worktree/HEAD attestation) — untouched.
- **Full 44-domain Canon semantic sign-off** — the roadmap's own Phase 2 ("static 44-domain parity reconciliation") is not marked complete anywhere; consistent with the implementation plan's own closing line that GAP-001 (semantic-parity sense) remains open for "every domain beyond the first two that has not received Domain-1-level depth."
- **Item 10** (Finance's structural accounting model / general ledger) — deliberately left as an open business question this session, not implemented. See `Finance-General-Ledger-Decision-Note-2026-09-16.md`.

---

## 4. Phase 4 categories — update, 2026-09-21

Being honest about scope: the original 2026-09-16 pass researched topology/deployment/backup/diagnostics, the finding register and Canon matrix, and domain-certification currency across Procurement/Inventory/Production/Settings/UserMgmt/Shipping/Finance/CRM/Customers/Personalization. Six categories were left unresearched. A follow-up pass on 2026-09-21 closed four of them; a second follow-up pass, same day, researched the remaining two plus monitoring (see below) — all six categories are now researched, though not all six gaps are closed.

**Closed, 2026-09-21:**

- **Authoritative-writer control** — the corpus itself had flagged this as unverified (`smp1-finding-register-reconciliation-2026-09-13.md`: "Current authority-state evidence remains unverified. Do not claim uncontrolled dual writers actually exist without evidence."). **Directly confirmed by the operator, 2026-09-21: the frozen Apps Script tool is not currently in live use — Standalone is the sole live system.** No dual-writer risk exists today. This closes the data-integrity question the corpus was right to flag; it does not by itself establish exactly when or how that became true (see the cutover-sequence draft below, which asks the operator to confirm which of three possible histories applies).
- **Cutover sequence** — no procedure existed anywhere in the corpus. Draft proposal written: `SMP1-Cutover-Sequence-DRAFT-2026-09-21.md`. Explicitly not authorized/certified — a starting point for the operator to correct, not a governance record.
- **Rollback/recovery procedure** — distinct from backup/restore (§3 above, which covers the *metadata model*); no actual "if something breaks, here's how to revert" procedure existed. Draft proposal written: `SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md`, which is explicit about inheriting the still-open backup/restore-verification gap rather than papering over it.
- **Support/escalation ownership** — no document existed; this is fundamentally an organizational decision, not a technical one. Draft proposal written: `SMP1-Support-Escalation-Ownership-DRAFT-2026-09-21.md`, proposing a tiered structure sized to the one confirmed user/administrator this system currently shows, with explicit open questions (especially: who owns Tier 2 code/deployment support, if anyone) rather than an invented answer.

**Researched, 2026-09-21 (second pass) — findings below. None of these three close the underlying gap; they replace "not researched" with an accurate, evidence-based picture of what the gap actually is.**

- **Migration mapping — correction to this document's own earlier claim above.** "No actual traceability content has ever been filled in for any domain" **overstated the gap** — the same kind of overstatement this pack itself was written to catch elsewhere in the corpus (see §1). `docs/canon-mapping/` genuinely is just a one-paragraph methodology statement, unchanged. But `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` contains real, structured field-level crosswalks for **three domains**, buried in audit prose rather than packaged as a mapping deliverable: Domain 1 (Orders) — a full 47-row `frozen column | entity property | Postgres column` table (lines 161–221); Domain 2 (Inventory/Material) — 23 fields, stated as "a clean, complete field-level match — no gaps"; Domain 3 (Production) — the first 12 fields matched, the rest explicitly flagged as not individually re-verified. Neither `docs/canon-mapping/` nor any of the 44 Evidence Cards link to this work — it is structurally invisible from the two places a reader would actually look. Even where it exists, it maps frozen ↔ Standalone only, not the full Canon ↔ frozen ↔ Standalone chain TITAN LOCK requires (Domain 2's own disposition: "a formal Canon-to-Standalone three-way capability table remain[s]... open"). **Corrected picture**: 3 of 44 domains have real (2 complete, 1 partial) frozen↔Standalone field mapping, undiscoverable from the mapping directory; the other ~41 have none. Building a real crosswalk for the rest, and cross-linking the existing three into `docs/canon-mapping/`, remains substantial new authorship — that part of the original claim holds.
- **Data and business reconciliation — researched, genuinely unresolved, not decided either way.** Verified directly: no data-import/ETL code exists anywhere in the repo (all 35 `migration/*.mjs` files are schema-only — tables/columns, not data loaders; no Sheets/Google-Data-API client dependency exists outside Google Sign-In auth). Canon explicitly requires historical-data preservation through migration and cutover (Stage 10 §10.33, per Domain 29's own evidence card), and Domain 29 is honest that this requirement is unmet: "Schema presence is not migration evidence... no data migration... is authorized in this wave." No document anywhere states whether Standalone was designed as fresh-start-only (frozen's history staying behind as a read-only archive) or was always meant to inherit frozen's data — that intent question is genuinely undocumented, not merely unresearched, and depends on the same cutover-history question (a)/(b)/(c) the cutover-sequence draft already asks the operator to resolve. **A new, separate finding surfaced during this research, not previously known**: Phase 3's acceptance evidence (`Phase-3-Fresh-Authenticated-ERP-Acceptance-2026-09-16.md`) shows the Customers page reporting "Total Orders: 3" for the same customer the Dashboard/Orders pages show has exactly 1 order — an unexplained discrepancy in live production data, not a documentation issue. This is newly flagged here; it was not caught in the original Phase 3 pass.

**Investigated and fixed, 2026-09-21 (working tree, not committed).** Root cause confirmed by direct code inspection: a customer's `total_orders` is a stored counter (`packages/platform/src/customer.ts`), incremented on order creation but never decremented anywhere, while the Dashboard/Orders figure is a live, unfiltered `COUNT` of the `orders` table (`apps/api/src/order-service.ts:747-749`). `apps/api/src/server.ts`'s `customerAwareOrderMutationPersistence.createWithoutActivity` resolved/upserted the customer (committing the increment) *before and separately from* the order insert's own transaction — a failed or retried insert afterward could leave the counter incremented with no matching order, consistent with 3 vs. 1. A second, independent, more serious defect was found while designing the fix: that same method was declared with only one parameter, silently dropping the `onCreated` side-effect resolver the coordinator layer passes through at runtime (TypeScript permits an implementation with fewer parameters than its declared interface, so this compiled cleanly). That resolver is exactly the mechanism item 7's certified CRM lead-to-order duplicate-conversion fix relies on to mark a lead converted atomically with its order — **meaning item 7's certified fix has not actually been executing in production**, despite being "CERTIFIED WITH CONTROL / CLOSED." Both are fixed together (same function): `packages/platform/src/order.ts`'s `createWithoutActivity` gained an optional `resolveCustomerId` parameter mirroring `updateWithoutActivity`'s existing one exactly, invoked inside the same transaction before the insert; `server.ts`'s wrapper now declares and forwards `onCreated` correctly, and passes customer resolution as a transaction-scoped resolver instead of an eager pre-call. `apps/api/test/customers-routes.test.mjs`'s architectural-boundary test was updated to match the corrected source shape (it previously asserted the buggy structure). Full monorepo build clean; regression suite re-run: domain 339/339, platform 233/233, api 666/667 (the one failure is the same pre-existing, unrelated `reports-routes.test.mjs` timezone issue tracked since PHB-1). **Committed (`babd230` docs, `15db3e9` the fix itself), pushed, and deployed to production, 2026-09-21** — both Render services (`gifthatkeos-standalone-v1-api`, `-web`) manually deployed to `15db3e9` and confirmed live: `/health` responding, the authenticated Dashboard loading real data (Total Orders: 1, matching production), and a repo-wide spot-check (full TypeScript AST walk) finding no other instance of the same dropped-parameter pattern anywhere in the monorepo. The stale customer record itself (`total_orders: 3`) was deliberately left uncorrected — confirmed still showing 3 on the live Customers page post-deploy, exactly as expected — the operator chose to fix the code defect only, not the existing bad data, on direct question.
- **Monitoring — researched. Verdict: nothing beyond Render's own basic infrastructure dashboard exists.** No APM, error tracker, remote log shipping, or alerting is configured anywhere (checked all `package.json` files across every workspace — no Sentry/Datadog/New Relic/OpenTelemetry/Prometheus/etc.; `render.yaml` has no notification/alert configuration). Fastify's built-in Pino logger runs in production (`logger: true`) but writes structured JSON to stdout only, captured by Render's own basic log viewer and nowhere else. A real diagnostics service exists (`packages/platform/src/diagnostics-builtins.ts` — four read-only checks, including a genuine Postgres health probe) but is deliberately instantiated-and-unpublished in `apps/api/src/server.ts` (`void diagnosticsApplicationService`) because the 109-key permission catalogue has no entry covering diagnostics/observability, and governance prohibits inventing one (`PACK_2_9B5B_DIAGNOSTICS_HTTP_AUTHORIZATION_DEFERRAL_DECISION.md`) — this is a deliberate, certified deferral, not an oversight. Domain 14 (Enterprise Monitoring, Observability & Operations) is classified "partial foundation," and its Canon-level requirements (SLA/SLO governance, incident governance, capacity management) have no repository counterpart at all beyond the diagnostics-provider skeleton.

---

## 5. What this pack directly resolves and what it doesn't

**Resolves (as of 2026-09-16):** a reader now has an accurate map of which of the ~165 governance/certification files are current, which are stale and why, and which capabilities are undocumented — closing the "does the documentation even reflect reality" question Phase 4 was checking for. The Handover Roadmap's §4 blocker table (both vault copies) was corrected directly to remove the stale GAP-005/006 entries.

**Resolves (as of 2026-09-21):** authoritative-writer control (§4) is now closed on real operator confirmation, not guesswork. Cutover sequence, rollback/recovery procedure, and support/escalation ownership each now have a draft proposal to react to instead of a blank page (`SMP1-Cutover-Sequence-DRAFT-2026-09-21.md`, `SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md`, `SMP1-Support-Escalation-Ownership-DRAFT-2026-09-21.md`) — none of these are authorized or certified; all explicitly ask the operator to confirm or correct specific open questions.

**Still does not resolve:** none of GAP-002, GAP-009, or GAP-011 are closed by this pack — they're described more precisely, not implemented. No new certification/closure records were written for the eleven items in §2 (the finding-register documents themselves, as opposed to the Roadmap, were also not corrected — only the Roadmap's blocker table was). No live backup/restore test was performed (the rollback draft explicitly inherits this as an open risk). Migration mapping, data/business reconciliation, and monitoring (§4) remain genuinely unresearched. No code changed at any point; nothing was committed or pushed as part of producing this pack or its 2026-09-21 follow-up.

**Recommended next actions, not authorized here:**
1. Delete or clearly mark-superseded the dead `.do/app.yaml`.
2. Write one superseding Render/Neon topology certificate, closing GAP-009's documentation half (the operational half — live backup/restore, monitoring — remains separately gated).
3. Correct the finding-register documents themselves (not just the Roadmap) for the GAP-005/006/008 entries.
4. Write closure records for the eleven items in §2, or explicitly retire the practice of per-capability certification docs in favor of the GAP-001 consolidated implementation plan as the ongoing source of truth (the finding-register agent's recommendation — the two numbering/documentation tracks shouldn't be left to silently coexist).
5. Review and correct the three 2026-09-21 draft runbooks, then decide whether to promote them from DRAFT to an authorized status.
6. Research migration mapping, data/business reconciliation, and monitoring before Phase 4 can be called genuinely complete.
7. Actually run the backup/restore drill the rollback draft recommends, against a disposable Neon branch, before relying on that procedure for real.

---

## 6. Documentation backlog status — update, 2026-09-21

Items 1–3 of §5's "Recommended next actions" are now done, per the same
TITAN LOCK additive-correction convention used throughout this corpus
(original content preserved; corrections appended as new dated sections,
never edited in place):

1. **Done.** `.do/app.yaml` now carries a "SUPERSEDED, 2026-09-21 — DO NOT
   USE" banner pointing to `render.yaml` and item 2 below. File otherwise
   unchanged.
2. **Done.** `docs/governance/smp1-standalone-v1-render-neon-production-
   topology-2026-09-21.md` written — a full superseding Render/Neon topology
   certificate, closing GAP-009's documentation half.
3. **Done.** Additive corrections appended to:
   `smp1-finding-register-reconciliation-2026-09-13.md` (and its vault
   mirror) for the GAP-005/006/008 disposition; the Domain 6 (Finance) row
   of `smp1-44-domain-parity-matrix-2026-09-13.md` (and its vault mirror,
   `SMP1-44-Domain-Parity-Final-Matrix-2026-09-13.md`), correcting the
   "closed read-only workspace" claim now that PHB-1/PHB-2 added real
   Finance write capability; and the BOM-consumption atomicity claim across
   `smp1-production-inventory-consumption-implementation-certification.md`,
   `-ownership.md`, and `-ownership-certification.md` (item 8's fix).

4. **Done.** Operator decided, on direct question: retire per-capability
   certification for the eleven items in §2, in favor of the GAP-001
   consolidated plan as their ongoing closure record. Decision recorded in
   `docs/governance/smp1-per-capability-certification-retirement-2026-09-21.md`;
   the GAP-001 plan itself was updated with a matching 2026-09-21 status
   section designating it as such (and correcting its own stale "not yet
   committed" markers and cross-reference section in the same pass).

5. **Done (review/correction pass).** All three 2026-09-21 draft runbooks
   reviewed and corrected: the cutover-sequence draft's GAP-009 line updated
   to reflect its documentation half closing (item 2 above); the
   rollback/recovery draft cross-referenced against the topology
   certificate's independent confirmation of the same backup/restore gap;
   the support/escalation draft given the topology cert's concrete service
   IDs for Tier 3 and a pointer to the retirement decision (item 4) for
   Tier 2. **Promotion from DRAFT to authorized status is still the
   operator's decision, not made here** — each draft's own open questions
   (which cutover-history scenario applies, the single-operator-scale
   assumption, the Neon plan's actual PITR capability, who if anyone owns
   Tier 2) still require the operator's confirmation.
6. **Done (research).** Migration mapping, data/business reconciliation,
   and monitoring all researched — see §4's second 2026-09-21 update above
   for full findings, including a correction to this pack's own earlier
   migration-mapping claim and a newly-surfaced live-data discrepancy
   (Customers-page vs. Dashboard/Orders order-count mismatch) neither
   researched nor resolved here.

7. **Done, 2026-09-21.** Ran a real restore drill against a disposable Neon branch (not production), via Claude in Chrome driving the operator's already-authenticated Neon console session — no credentials handled directly. Confirmed: Free plan, 6-hour PITR history window, enforced exactly (an out-of-window branch attempt was rejected with an explicit error naming the precise earliest restorable point); branch-from-point-in-time creation works, is fast (~3 sec), and produces a genuine, correct, queryable copy (spot-checked order count against production). Also found two new facts: the console's "Preview data" feature errors consistently (branch-then-query is a reliable workaround), and Neon's separate snapshot feature is unconfigured — meaning recovery capability today is strictly bounded to the rolling 6-hour window, with no backstop beyond it. **Not verified**: actually promoting a restored branch to replace production (updating Render's `DATABASE_URL`/`DATABASE_MIGRATOR_URL` and redeploying) — a materially larger, untested step, now explicitly flagged as the next thing to verify. Full writeup and the updated procedure: `SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md`. Drill branches created and deleted cleanly; no residue.

**All seven items of the documentation backlog are now addressed** — six closed outright, one (the production-cutover promotion step inside item 7's own procedure) explicitly identified as the next thing to verify rather than assumed safe.
