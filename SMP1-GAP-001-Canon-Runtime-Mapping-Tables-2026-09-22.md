# SMP1 GAP-001 — Canon-to-Runtime Lifecycle Mapping Tables

**Date:** 2026-09-22
**Status:** DRAFT PROPOSAL — not yet reviewed or authorized by the operator. This is the follow-on documentation work the lifecycle-vocabulary decision brief's "document a mapping" decisions (Orders, Production, Shipping, Finance transactions) called for; writing the tables is not itself an authorization to treat them as final.
**Origin:** `SMP1-GAP-001-Canon-Runtime-Lifecycle-Vocabulary-Decision-Brief-2026-09-22.md`, decisions recorded 2026-09-22. Four of that brief's eight domains were decided as "document a mapping" rather than "amend Canon." This is that documentation.

## How to read these tables, and their real limitation

Each table maps Canon's named abstract stages onto the concrete runtime statuses both frozen and Standalone already implement (identically, in every domain below). Where a Canon stage clusters multiple runtime statuses, that's a real interpretive judgment about which concrete states belong under which abstract milestone — not a fact independently verified against a transition-order specification, since Canon's Stage 10 documents name states without necessarily specifying inter-state transition rules at the same level of detail as the certified codebases do. **Confidence varies by row, marked explicitly** — some mappings are close to mechanical (an exact name match), others are a defensible-but-real judgment call this document is making for the first time, not re-deriving from an existing authoritative source. Treat the marked judgment calls as the actual content requiring operator review, not the exact-match rows.

Confidence key: **Exact** (identical name, high confidence) · **Strong** (same concept, different name, high confidence) · **Judgment call** (a real interpretive choice, flagged for review) · **Unmapped** (no runtime equivalent exists at all, recorded honestly rather than forced).

---

## 1. Orders (Domain 1)

Canon's 5 core stages, mapped onto the 18 runtime statuses (frozen = Standalone, exact vocabulary match between the two systems):

| Canon stage | Runtime status(es) | Confidence |
| --- | --- | --- |
| Quotation | Draft | Judgment call — the pre-confirmation state is the natural "not yet a real order" analog, but Canon doesn't specify a literal quotation document, and none exists in either codebase |
| Pending Approval | Confirmed, Waiting for Details, Ready for Personalization, Personalization, Customer Approval | Judgment call — reads as the whole pre-production preparation phase, not narrowly "awaiting one approval." `Customer Approval` is the strongest single match inside this cluster |
| Production Planning | Ready for Production, In Production | Strong |
| Quality Inspection | Quality Check | Exact |
| Closed | Ready for Packing, Packed, Ready to Dispatch, Dispatched, Delivered | Judgment call — Canon's single "fulfillment complete" bucket covers five distinct operational states; `Delivered` is the natural terminal representative if a single status must stand for the cluster |

**Runtime-only states, not covered by any Canon core stage:** On Hold, Cancelled, Returned, Refunded. Canon names one exception state for this domain — **Archived** — which is tracked separately via the `recordStatus` field, not as an `ORDER_STATUSES` value, consistent with how every other domain in this report handles archival. On Hold/Cancelled/Returned/Refunded have no Canon-named counterpart at all; they're genuine runtime-only operational exceptions.

---

## 2. Production & Manufacturing (Domain 3)

Canon's 10 core states + 5 exceptions, mapped onto the 9 runtime stages (frozen = Standalone).

**A note before the table, worth taking seriously**: Canon's stated core-state order is Requested → Planning → Scheduled → Ready → Released → In Progress → ..., with Scheduled *before* Ready. The runtime's own declared order is Awaiting Handoff, **Ready, Scheduled**, Machine Assigned, In Production, ... — Ready *before* Scheduled, the reverse. `Scheduled` and `Ready` are exact *name* matches (already established in the original pass), but this draft has **not independently confirmed** they play the same *sequence* role — that would need checking against `PRODUCTION_ALLOWED_TRANSITIONS`/`productionRulesCanEnter_` directly, not inferred from two state-name lists. Flagging this explicitly rather than silently assuming name-match implies sequence-match.

| Canon stage | Runtime stage(s) | Confidence |
| --- | --- | --- |
| Requested | Awaiting Handoff | Judgment call |
| Planning | *(no clean equivalent)* | Unmapped — absorbed loosely into Awaiting Handoff/Ready, or genuinely uncovered |
| Scheduled | Scheduled | Exact (name) — sequence position not independently verified, see note above |
| Ready | Ready | Exact (name) — sequence position not independently verified, see note above |
| Released | Machine Assigned | Judgment call — "released to begin" reads naturally as "a machine has been assigned" |
| In Progress | In Production | Strong |
| Production Complete | Completed | Strong |
| Quality Handover | Quality Check | Judgment call — the original pass explicitly flagged these as *different concepts* ("Quality Handover" a transition event, "Quality Check" an inspection state), not a clean match; recorded here as the best available runtime analog anyway, per the operator's "document a mapping" decision |
| Accepted | Ready for Packing | Judgment call — "accepted" reads as "QC passed, cleared to move toward packing" |
| Closed | *(no clean equivalent)* | Unmapped — the job appears to close implicitly when the parent Order progresses past Ready for Packing, not via its own distinct Production-stage value |

**Exceptions:** Rework → Rework (exact). On Hold, Cancelled → per the operator's own decision, inherited from the parent Order's equivalent states rather than tracked at the Production-job level. **Aborted → genuinely unmapped**, and worth flagging even though the operator's decision covered On Hold/Cancelled by inheritance — "Aborted" (a job stopped mid-way, after starting) is conceptually distinct from "Cancelled" (never started, or cancelled before/without production-specific handling), and the inheritance reasoning doesn't obviously cover it. Archived → tracked separately via Record Status, same pattern as every other domain.

---

## 3. Shipping & Fulfillment (Domain 5)

Canon's 9 states, mapped onto the 17 runtime statuses (frozen = Standalone, cross-confirmed against live production data). This is the strongest-alignment domain in the set — 5 of 9 Canon states are already exact matches.

| Canon stage | Runtime status(es) | Confidence |
| --- | --- | --- |
| Planned | Created | Judgment call — the shipment record's initial creation is the natural "planned" moment |
| Allocated | Ready To Pack | Judgment call — presumably resources/stock are allocated by the time a shipment is ready to pack |
| Packing | Packing | Exact |
| Packed | Packed | Exact |
| Carrier Assigned | Awaiting Courier, AWB Generated | Strong — already identified in the original pass as Canon's one state split into two finer runtime steps |
| Dispatched | Dispatched | Exact |
| In Transit | In Transit | Exact — Picked Up, Reached Hub, and Out For Delivery are finer runtime-only refinements of this same Canon milestone, not separate Canon stages |
| Delivered | Delivered | Exact |
| Closed | *(no distinct equivalent)* | Unmapped — the runtime appears to treat Delivered as terminal, with no separate post-delivery reconciliation/closure status |

**Runtime-only exception states, not covered by any Canon core stage:** Delivery Failed, Customer Unavailable, RTO Initiated, Returned, Cancelled. Consistent with the absent-governance brief's separate finding (formally descoped) that Canon's Return Request/Shipment/Case entities don't exist as their own governed things in either system — these five statuses are exactly where that gap surfaces at the vocabulary level.

---

## 4. Finance — Transaction Lifecycle (Domain 6)

Canon's 7 states, mapped onto the runtime's 3 values (Posted, Pending, Reversed). **This is the most compressed mapping in the set and the one most worth a second, accounting-literate look before treating as final** — the brief itself flagged Finance as the one domain where the stakes are explicitly higher (external audit/tax/lender exposure) and recommended an accountant's input, not just an engineering one.

| Canon stage | Runtime value | Confidence |
| --- | --- | --- |
| Initiated | Pending | Judgment call — real compression, see below |
| Validated | Pending | Judgment call — same bucket as Initiated; no distinct runtime state exists for "validated but not yet approved" |
| Approved | Pending | Judgment call — same bucket again; three distinct Canon pre-posting stages collapse into one runtime status |
| Recognized | Posted | Judgment call |
| Posted | Posted | Exact |
| Settled | Posted | Judgment call — same bucket as Recognized/Posted; no distinct runtime state exists for "posted but not yet settled" |
| Closed | *(no distinct equivalent)* | Unmapped — a Posted transaction appears to stay Posted permanently, no further closing step |

**Reversed → genuinely does not map to any Canon-named state**, and this is a structural difference, not just a missing label. As the original pass noted directly: Canon models corrections through a separate "Financial Adjustments" entity, not as a status value on the original transaction. The runtime's `Reversed` status represents the same real-world event (a correction) through a different mechanism (a status flag on the existing record) than Canon specifies (a distinct linked entity). Recording a mapping table entry for `Reversed` would misrepresent this as a vocabulary gap when it's actually a data-model difference — flagged here rather than force-fit into the table above.

**Honest summary of this domain's compression**: three of Canon's seven states (Initiated, Validated, Approved) collapse into one runtime value (Pending); two more (Recognized, Settled) collapse into Posted alongside Posted itself; Closed has no representation at all; and Reversed sits entirely outside Canon's modeling approach for this concept. This is a real, substantial compression — the mapping documents what exists, it doesn't argue the compression is harmless. That judgment (whether it matters for GiftHatke's actual audit/tax/lending exposure) is exactly the accounting-literate review this table is meant to be reviewed against, not a substitute for it.

---

## Status and next step

These four tables are a first draft, written to fulfill the specific follow-on task the lifecycle-vocabulary brief's "document a mapping" decisions created — they are not yet reviewed, corrected, or authorized by the operator. Several rows are explicitly flagged as judgment calls or genuinely unmapped rather than presented as settled fact. Once reviewed (corrected, confirmed, or partially revised), this document's status line should be updated to CERTIFIED and it becomes the authoritative Canon-to-runtime mapping record for these four domains, closing the last open procedural step from either GAP-001 decision brief.
