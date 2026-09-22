# SMP1 GAP-001 — Canon-to-Runtime Lifecycle Vocabulary: Decision Brief

**Date:** 2026-09-22
**Status:** DECISIONS RECORDED, 2026-09-22 (Hitendra Chug) — see each domain below. 4 of 8 domains document a mapping (Orders, Production, Shipping, Finance), 4 of 8 amend Canon (Materials, Users, CRM, Products), 0 authorized to build. No code changes result from this brief by design — every decision was (A) or (B), never (C).
**Origin:** `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md`'s closing statement (2026-09-22), after the 44-domain sweep and this session's three code/infrastructure loose ends (REORDER_POLICIES, OrderItem Category/Product Type, the production-cutover rehearsal) were all closed. That statement named the vocabulary/lifecycle disagreements below as the one category of remaining GAP-001 blocker this read-only reconciliation report was never authorized to resolve on its own.

## What this brief is, and isn't

Across every domain reviewed this session, one pattern held without exception: **frozen and Standalone agree with each other exactly.** Not once did this report find Standalone diverging from frozen's own operational vocabulary. The mismatch documented below is entirely between what both systems actually implement (identically) and what the 44-Domain Canon's Stage-10 documents separately describe. This is not a bug in either codebase — it's an unresolved question of which vocabulary is authoritative, and it has sat unresolved since before this migration began.

**This brief covers only lifecycle-vocabulary mismatches** — cases where Canon names a specific multi-state lifecycle for an entity that both systems already implement, just with different state names or fewer states. It does **not** cover the separate, larger category of Canon-named governance capabilities that don't exist as their own thing in *either* codebase at all — Quality Management (Domain 4), Workflow/BPM (Domain 11), Master Data Management (Domain 12), API/Contract Governance (Domain 13), Sales/Quotation governance (Domain 20), Marketing/Campaign governance (Domain 22), and similar. Those are "build it or formally descope it" business decisions, a different kind of call than "which vocabulary do we use," and worth their own separate brief if useful — flagging that scope boundary here so this document isn't mistaken for a complete GAP-001 closure checklist.

## The three resolution paths

For each domain below, one of these three applies (or some mix, decided per named state):

- **(A) Document a mapping.** Certify that the runtime vocabulary already satisfies Canon's abstract stage, with an explicit Canon-state → runtime-state table recorded in the Canon reconciliation record. No code changes. Reversible in the sense that it's a documentation decision, not a schema commitment.
- **(B) Amend Canon.** Treat Canon's Stage-10 text as pre-implementation vocabulary, superseded by what was actually built and already certified as CERTIFIED/LIVE/CLOSED in several of these domains. Formally update Canon's own wording to match reality, rather than the other way around.
- **(C) Build the missing states.** Treat a named-but-absent state as a real, unmet business requirement and implement it — new code, a new migration, new UI. The only path that changes the running system, and the one hardest to justify under SMP1's own stated premise (parity to frozen, not new features).

Nothing below recommends one path per domain. The stakes and plausibility differ enough by domain that a single blanket answer ("always A") would paper over real differences — that judgment is the point of this brief.

---

## Domain 1 — Orders

**Canon (§10.6):** 5 core stages — Quotation, Pending Approval, Production Planning, Quality Inspection, Closed — with Archived as an exception state.

**Frozen = Standalone (18 states, exact match between the two systems):** Draft, Confirmed, Waiting for Details, Ready for Personalization, Personalization, Customer Approval, Ready for Production, In Production, Quality Check, Ready for Packing, Packed, Ready to Dispatch, Dispatched, Delivered, On Hold, Cancelled, Returned, Refunded. Archived is tracked separately via a `recordStatus` field, not as an `ORDER_STATUSES` value.

**The gap:** Canon's 5 abstract stages plausibly cluster the 18 concrete statuses (e.g. "Production Planning" could span Ready for Production → In Production), but no one has ever written that clustering down. "Canon-to-runtime semantic mapping remains unresolved" — this report's own words from the pass that found it.

**Consideration:** Orders is the domain with the most granular, clearly-purposeful runtime vocabulary of any reviewed (18 distinct operational states, each doing real work in the business — personalization hand-off, QC, dispatch). Path (A) looks like the natural fit; the runtime states almost certainly *are* Canon's abstract stages, just named at operational rather than governance granularity.

**Decision, 2026-09-22 (Hitendra Chug): Document a mapping.** Certify the 18 runtime statuses satisfy Canon's 5 abstract stages via an explicit Canon-stage → runtime-status table. No code changes.

---

## Domain 2 — Materials (Inventory)

**Canon (§10.6):** 7 lifecycle states — Draft, Review, Approved, Active, Suspended, Retired, and one further named state.

**Frozen = Standalone:** A single boolean `Active`/inactive flag. No enumerated status list exists anywhere in either codebase.

**The gap:** The starkest of the eight domains reviewed. Five of Canon's seven named states — Draft, Review, Suspended, Retired, and the seventh — have **no corresponding concept in either system at all**, not even an informal one. This isn't a naming mismatch; it's a genuine capability absence.

**Consideration:** Unlike Orders, this isn't "the runtime vocabulary is Canon's vocabulary in different words" — a boolean genuinely cannot represent a 7-state lifecycle. Path (A) alone doesn't really work here; it would require deciding that Canon's richer material-governance lifecycle (draft materials under review, formally retired materials kept for audit trail, etc.) simply isn't a GiftHatke business need at current scale — which is closer to path (B), amending Canon to reflect that this level of material governance was never actually required. If it *is* a real need, this is the one candidate in this brief most likely to warrant path (C).

**Decision, 2026-09-22 (Hitendra Chug): Amend Canon.** This level of material-governance lifecycle (draft/review/suspend/retire workflow) was never actually required at current scale. Canon's 7-state text to be updated to reflect the boolean model actually built and certified. No implementation authorized.

---

## Domain 3 — Production & Manufacturing

**Canon (§10.6):** 15 named states — 10 core (Requested → Planning → Scheduled → Ready → Released → In Progress → Production Complete → Quality Handover → Accepted → Closed) plus 5 exceptions (On Hold, Rework, Cancelled, Aborted, Archived).

**Frozen = Standalone:** 9 stages — Awaiting Handoff, Ready, Scheduled, Machine Assigned, In Production, Quality Check, Rework, Completed, Ready for Packing. Archived tracked separately via Record Status.

**The gap:** Only 3 exact name matches (Scheduled, Ready, Rework). Several Canon states are entirely absent from the runtime vocabulary with no near-synonym at all — On Hold (present in Orders, missing here), Cancelled, Aborted, Requested, Planning, Released, Accepted, Closed. Called out in the original pass as "the most divergent vocabulary found so far."

**Consideration:** Some of the "missing" states may be legitimately redundant with Orders' own On Hold/Cancelled (a production job doesn't need its own Cancelled state if cancelling the order it belongs to already halts production) — that's a real path-(A)-shaped argument, but it needs someone who understands the actual production floor workflow to confirm, not just a vocabulary comparison.

**Decision, 2026-09-22 (Hitendra Chug): Document a mapping.** Certify the 9 runtime stages satisfy Canon's 15, with Order-level On Hold/Cancelled understood to cover the production-level equivalent by inheritance. No code changes.

---

## Domain 5 — Shipping & Fulfillment

**Canon (§10.6):** 9 states — Planned → Allocated → Packing → Packed → Carrier Assigned → Dispatched → In Transit → Delivered → Closed.

**Frozen = Standalone (17 states, cross-confirmed against live production data — `/shipping/workspace` reported exactly 17 statuses in production):** Created, Ready To Pack, Packing, Packed, Awaiting Courier, AWB Generated, Dispatched, Picked Up, In Transit, Reached Hub, Out For Delivery, Delivered, Delivery Failed, Customer Unavailable, RTO Initiated, Returned, Cancelled.

**The gap:** The best alignment of any domain in this brief — 5 of 9 Canon states match exactly (Packing, Packed, Dispatched, In Transit, Delivered). The 12 runtime-only states read as India-specific courier/logistics granularity layered *on top of* Canon's milestones (RTO — Return to Origin — is standard Indian COD e-commerce terminology with no Canon equivalent), not orthogonal to them.

**Consideration:** This is close to a solved case already — path (A) should be close to mechanical here, since most of Canon's named milestones are directly present as recognizable checkpoints. Included in this brief mainly for completeness and as a contrast case to Domain 2.

**Decision, 2026-09-22 (Hitendra Chug): Document a mapping.** Certify the 17 runtime statuses satisfy Canon's 9, with the 12 India-specific courier-granularity states recorded as refinements layered on top of Canon's milestones. No code changes.

---

## Domain 6 — Finance (Transaction Lifecycle)

**Canon (§10.6):** 7 states — Initiated → Validated → Approved → Recognized → Posted → Settled → Closed.

**Frozen `FINANCE_TRANSACTION_STATUSES`:** 3 values — Posted, Pending, Reversed. (Standalone's own vocabulary was not independently re-derived in the pass that found this; mirroring is assumed, consistent with every other domain, but not freshly re-confirmed for this specific list.)

**The gap:** Only `Posted` matches a Canon name exactly. `Pending` and `Reversed` have no Canon-named counterpart — Reversed is a legitimate accounting concept, but Canon models corrections through a separate "Financial Adjustments" entity rather than a transaction-status value.

**Consideration — the one place in this brief where the stakes are explicitly higher than the others:** Finance is the domain most directly exposed to external scrutiny — tax authorities, lenders, auditors — of any reviewed. The original pass flagged this as a higher-priority item for that reason alone, without taking a position on whether the compressed lifecycle is actually a problem (a single-entry, well-applied transaction log can still produce correct totals). Worth deciding this one with an accountant's input, not just an engineering one, if that's available. Separately and not addressed by any of the three paths above: neither system has Journal Entries, Journal Lines, or General Ledger Accounts at all, which Canon also certifies for this domain — that's a governance-layer-absent finding (the other category this brief excludes), not a vocabulary mismatch, but it lives in the same domain and the same original pass, so it's worth knowing about when this one comes up for decision.

**Decision, 2026-09-22 (Hitendra Chug): Document a mapping.** Certify the 3-value system (Posted/Pending/Reversed) satisfies Canon's 7-stage transaction lifecycle via an explicit mapping, notwithstanding the real compression. No code changes. (The separate Journal/GL absence — see the companion absent-governance brief — was formally descoped independently.)

---

## Domain 8 — Identity, Security & Administration (Users)

**Canon (§10.6):** 6 states — Requested → Verified → Provisioned → Active → Suspended → Archived.

**Frozen `Users` entity:** A `Status` field checked only as `'active'`/not — effectively a binary Active/Inactive flag, no enumerated multi-value list. (Standalone's own user-status vocabulary was not independently checked in the pass that found this.)

**The gap:** Same lifecycle-compression pattern as Materials — Canon's distinct provisioning/verification stage before activation, and its distinct Suspended-short-of-Archived state, have no equivalent.

**Consideration:** Practically, "can an admin deactivate a user account" already exists (the Active/Inactive flag does that job). What's genuinely missing is any notion of an account being *requested but not yet provisioned*, or *verified but not yet active* — onboarding-workflow states, not access-control states. Whether that onboarding granularity matters depends on how user accounts actually get created today (a question about process, not code, that this report didn't investigate).

**Decision, 2026-09-22 (Hitendra Chug): Amend Canon.** Onboarding-workflow granularity (requested-but-not-provisioned, verified-but-not-active) was never a real need; the existing Active/Inactive access-control flag is sufficient. Canon's 6-state text to be updated accordingly. No implementation authorized.

---

## Domain 19 — CRM (Lead Lifecycle)

**Canon (§10.6):** 5 states — Captured → Qualified → Assigned → Engaged → Converted.

**Frozen `CRM_STAGES` (real, substantial code on both sides — this isn't an absence, it's a genuine divergence):** New Enquiry, First Response Sent, Requirement Understood, Trust Building, Payment Requested, Payment Pending, Payment Received, Order Created, Lost.

**The gap:** **Zero of Canon's 5 named states have an exact match** — the sharpest complete mismatch in this brief, and one of only two zero-match results found across the entire 44-domain sweep (the other being an entire domain with no implementation at all, a different category). Frozen's stages describe a GiftHatke-specific, payment-centric sales funnel; Canon's describe a generic qualification funnel. Even loose conceptual pairing is hard: Canon's "Assigned" is a funnel stage, while frozen's assignment is a separate `Assigned To` field, not a position in the stage sequence; "Qualified" and "Engaged" have no clean frozen counterpart at all.

**Consideration:** This looks like the clearest candidate for path (B) in the whole brief. The frozen funnel isn't a worse or incomplete version of Canon's — it's a different, GiftHatke-specific model built around how this business actually converts leads to paid orders, already CERTIFIED/LIVE/CLOSED as its own thing. Forcing a mapping onto Canon's generic funnel (path A) would likely distort more than it clarifies; the more honest move may be amending Canon's lead-lifecycle text to reflect the funnel that was actually built and that generates real revenue today.

**Decision, 2026-09-22 (Hitendra Chug): Amend Canon.** Frozen's 9-stage, payment-centric funnel is the deliberate, revenue-generating, GiftHatke-specific model actually in use — not an incomplete version of Canon's generic funnel. Canon's 5-stage text to be updated to reflect it. No implementation authorized.

---

## Domain 21 — Product Lifecycle

**Canon (§10.6):** 8 states — Concept → Defined → Validated → Approved → Active → Modified → Retired → Archived.

**Frozen `PRODUCT_HEADERS`:** A single boolean `Active` field — same single-flag compression as Materials, Production jobs, Financial Transactions, and Users.

**The gap:** Same pattern as Materials — most of Canon's named lifecycle states have no runtime equivalent at all. **However**, unlike Materials, the surrounding *resource model* (not the lifecycle) is a comparatively good match: `PRODUCT_HEADERS` already carries Category, Product Type, Variant Name, Personalization Fields, and Image URL, which map reasonably well onto Canon's Product Category/Variant/Personalization Rule/Digital Asset Reference concepts, even without their own formal typed entities. (This session separately closed a related, narrower gap: Standalone's `OrderItem` genuinely lacked Category/Product Type fields that this resource model implies it should carry — now fixed, unrelated to the lifecycle-state question here.)

**Consideration:** The lifecycle question here is essentially the same call as Materials (a boolean can't represent an 8-state lifecycle; decide whether that richer governance is a real need or not), but the resource-model question is more solved already than Materials' equivalent — worth deciding them somewhat independently even though they're both filed under "Domain 21."

**Decision (lifecycle), 2026-09-22 (Hitendra Chug): Amend Canon.** Same reasoning as Materials — this level of product-governance lifecycle (draft/validated/approved/retired workflow) was never actually required. Canon's 8-state text to be updated to reflect the boolean model actually built. No implementation authorized. (The resource-model question — Category/Product Type on OrderItem — was a separate, already-closed code gap, not part of this decision.)

---

## Decision authority

Following the same standing established for the recovery procedure (`SMP1-Rollback-Recovery-Procedure-DRAFT-2026-09-21.md` §3): **Hitendra Chug**, sole operator, no delegation or secondary approver.

## After a decision is recorded

All eight decisions above are now recorded (2026-09-22, Hitendra Chug). Propagated into `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` and `docs/governance/smp1-44-domain-parity-matrix-2026-09-13.md` (Standalone repo) as closing notes, matching the voice and structure already established throughout both documents. No code changes result: every decision was (A) document a mapping or (B) amend Canon, never (C) build.

**What this brief's completion means for GAP-001**: combined with the companion absent-governance-capabilities brief (also fully decided, 2026-09-22), both categories of GAP-001 blocker this report's closing statement identified are now decided — every remaining item has an explicit, dated, attributed operator decision on record rather than sitting as an open question. Whether that constitutes GAP-001's formal closure is a call for whoever holds that report's own closure authority, not something either brief closes on its own by being filled in.
