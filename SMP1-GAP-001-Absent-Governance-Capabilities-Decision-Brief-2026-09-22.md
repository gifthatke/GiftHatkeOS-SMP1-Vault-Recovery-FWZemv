# SMP1 GAP-001 — Absent Governance Capabilities: Decision Brief

**Date:** 2026-09-22
**Status:** OPEN — decisions requested below, per domain. Nothing in this brief is authorized, and no code, Canon text, or matrix classification changes as a result of writing it.
**Origin:** Companion to `SMP1-GAP-001-Canon-Runtime-Lifecycle-Vocabulary-Decision-Brief-2026-09-22.md`, which explicitly scoped out this category and pointed here. Where that brief covers cases where both systems implement an entity but name its lifecycle differently from Canon, this brief covers cases where Canon names an entire governed capability — a domain, a resource-model family, a decision-workflow — that **does not exist as its own thing in either frozen or Standalone at all.**

## What this brief is, and isn't

Every gap below shares one structural fact, verified directly in each case, not assumed: **frozen never built the capability Canon names, so Standalone had nothing to inherit or port.** This is a Canon-to-frozen gap in every instance, not a Standalone-introduced regression — the same finding this report made about every domain in the companion brief, just for capabilities rather than vocabulary.

This is also, explicitly, a different *kind* of decision than the companion brief's. That brief asked "which vocabulary is authoritative" — a documentation question with low cost either way. This one asks "should this capability actually get built" — a real scoping-and-cost question, closer to a product-roadmap decision than a reconciliation exercise. The three resolution paths below reflect that.

**Relevant context, not a recommendation**: SMP1's own governing text is explicit and has been consistent throughout this entire migration — `SMP1_STAGE_2_FINAL_CERTIFICATION_AND_CONTROLLED_CLOSURE.md`: *"No redesign is authorized by this certificate. No Domain 45 is created."* SMP1's mandate was parity with frozen, not completion of Canon. That doesn't answer any item below on its own — some of these may be real, current business needs regardless of what frozen ever built — but it's the relevant baseline: silence on any of these is not an oversight, it's the default outcome of a parity-scoped migration, unless a decision below actively changes that.

## The three resolution paths

- **(A) Build it.** Treat Canon's certified capability as a real, current business need independent of what frozen has. Scope, cost, and schedule it as its own initiative — this is genuinely new work, not a migration task, and should be estimated and prioritized as such.
- **(B) Formally descope.** Record explicitly that this capability was never required for SMP1 (frozen never built it either) and isn't planned. This closes the item without building anything — the correct default under SMP1's own parity mandate unless something below argues otherwise.
- **(C) Rely on the lighter substitute already in place.** A few of these have a real, working, narrower mechanism that may already serve the actual business need even though it doesn't match Canon's fuller named model (Domain 9's Notification governance is the clearest example). Decide whether the substitute is sufficient, or whether the gap to Canon's model still matters.

---

## Domain 4 — Quality Management

**Canon (§10.4–§10.7):** An independently governed domain with its own 7-stage lifecycle (Planned → Assigned → In Progress → Evidence Complete → Evaluation Complete → Disposition Issued → Closed) and a 16-item resource model — Quality Standards, Inspection Plans, Inspections, Checklists, Quality Characteristics, Inspection Results, Quality Evidence, Non-Conformances, Defects, Corrective Actions, Preventive Actions, Rework Requests, Rework Verification, Quality Dispositions, Quality Certificates, Supplier Quality Records, Audit Findings.

**What exists, in both systems identically:** A lightweight QC mechanism embedded *inside* Production — a 4-value result (Pending/Passed/Failed/Rework Required) and a fixed 7-item checklist, gating whether a production job can complete. Standalone reproduces this exactly (`production.ts`, confirmed field-for-field).

**The gap:** A word-boundary-accurate search for CAPA, Non-Conformance, Disposition, and Quarantine returns **zero matches in either codebase** — not a weaker version of these concepts, an absent one. Called out in the original pass as "the starkest Canon-compliance gap found" among the domains reviewed at that point, despite Quality carrying the same `Implementation Readiness: Approved` certification mark as Order Management, Production, and Inventory & Procurement in Canon's own text.

**Consideration:** The embedded Production QC mechanism may be doing real work already (nothing in this report suggests defective products are shipping) — the question is whether that's sufficient, or whether the business has actually needed CAPA/non-conformance tracking and has been working around its absence some other way (a spreadsheet, a WhatsApp thread) that a real system should replace.

**Decision:** _______________

---

## Domain 7 — Reporting & Business Intelligence (governance layer only — the operational layer is already substantially built)

**Scope note, important:** this domain's *operational* capability — producing reports, dashboards, per-module analytics — was largely closed this migration (PHB-6 Executive Dashboard, PHB-7's six Business Intelligence modules, both shipped and deployed). What's covered here is narrower and still open: the *governance* layer Canon separately names around that operational capability.

**Canon (§10.6):** A KPI lifecycle — Proposed → Defined → Validated → Certified → Published → Monitored — governing KPIs, Metrics, Business Measures, Business Dimensions, Scorecards, Analytical Datasets, Reporting Catalogs, and Alert Rules as their own permanently-owned, governed entities.

**What exists:** Reports compute live from other domains' data on each call, with no persistence layer of their own — no stored KPI/metric/scorecard definition, no formal validation/certification/publication workflow. A repeat check this session, run specifically after PHB-7's real per-module Intelligence build landed, confirmed **zero matches** for any KPI-lifecycle concept — the large, real operational build did not incidentally satisfy this narrower governance requirement.

**The gap:** The numbers Reports produces are real and computed correctly (per the domains already reviewed); what's absent is any notion of a *governed catalog* of business metrics with their own definition/validation/certification lifecycle, separate from the code that happens to compute them today.

**Consideration:** Lower apparent business risk than most items in this brief — this is about *how metrics get defined and published*, not about the metrics being wrong. Worth deciding low on priority relative to the others here unless a specific governance need (e.g., an investor or auditor asking "who certified this KPI definition") has already surfaced.

**Decision:** _______________

---

## Domain 9 — Notification, Communication & Collaboration (partial — Notification governance is real; Collaboration is not)

**Canon (§10.4–§10.5):** Communication Templates, Escalations, Communication Preferences (under "Collaboration Governance"), plus Conversations, Messages, and Collaboration Workspaces as distinct owned entities.

**What exists:** Better than most domains in this brief. `ERP89NotificationCommunication*.gs` implements real, administrable Channels, Internal/Email delivery Rules, WhatsApp settings, Escalations (with configurable delay minutes and escalation level), Reminders, Templates, and Preferences — a genuine, non-trivial match to three of Canon's named entities, as configuration catalogs rather than runtime instances.

**The gap:** Conversations, Messages, and Collaboration Workspaces — a comments/threads/shared-workspace layer — have no corresponding sheet, service, or file under those or similar names in either system.

**Consideration:** This is the brief's clearest path-(C) candidate. The notification/escalation/template machinery is real and reasonably complete; only the conversational/threading layer is missing. Worth asking concretely: does anyone on the team currently need in-app comment threads on an Order, or does WhatsApp/email already fill that role well enough that building one would be redundant?

**Decision:** _______________

---

## Domain 11 — Workflow, Process Automation & Orchestration (narrower than it first looked — the Task/Work Board piece is now real)

**Scope note, important:** an earlier characterization of this whole domain as "aspirational, neither side has it" was corrected mid-session: frozen's Task/Work Board (`TaskService.js`/`WorkService.js`) turned out to be real, live, everyday-use functionality — the literal first screen every frozen employee sees on login — and Standalone's equivalent (Today's Work) was built and deployed this same session. That piece is closed. What's covered here is what's left: the broader workflow/BPM architecture.

**Canon (§10.4–§10.7):** Business Processes, Workflow Definitions, Workflow Instances with a formal Created → Running → Waiting → Completed lifecycle, Automation Rules, SLA/Escalation Policies, Workflow Templates, Audit Records, and AI-assisted Process Intelligence.

**What exists:** Frozen's nominally "universal" `WorkflowService.js` registers exactly one entity type (Shipment) and has exactly one caller anywhere in the codebase. Four other modules (Orders, Production, Procurement, and a second, separate copy for Shipment) each independently hand-roll their own transition tables rather than using it. Standalone's Procurement reproduces the same module-local pattern. Neither system has an actually-adopted generic workflow engine — frozen's nominal one is single-purpose in practice, not a real instance of the broader architecture.

**The gap:** No persisted Workflow Instance, Automation Rule, SLA/Escalation Policy, Workflow Template, or Audit Record entity exists anywhere in either codebase, distinct from (and much larger than) the Task Governance piece that's now closed.

**Consideration:** This is the most architecturally ambitious item in this brief — a real generic workflow engine is a substantial build, not a small one. Worth asking whether the actual business need is "more places need a Task-Board-style human-task flow" (a smaller, more tractable ask, closer to extending what Today's Work already does) versus "we need configurable multi-step automation with SLA escalation" (a much bigger one) before deciding a path.

**Decision:** _______________

---

## Domain 12 — Master Data Management

**Canon (§10.4–§10.7):** A centralized golden-record governance layer — stewardship, taxonomy, cross-domain duplicate prevention — sitting above the domains that consume Customer, Supplier, Product, Material, Equipment, Warehouse, and other master data.

**What exists:** Each module owns and manages its own master data independently — Customers within CRM/Customer, Materials within Inventory, Products within Orders — with no separate centralized layer. A search for "steward," "duplicate prevention," and "taxonomy" returns zero matches in frozen. A separate, targeted check this session specifically ruled out hidden customer-deduplication logic as a partial implementation — genuinely absent, not just unnamed.

**The gap:** No cross-domain stewardship, golden-record resolution, or duplicate-detection mechanism exists anywhere.

**Consideration:** This is the domain in this brief most likely to matter at *scale* rather than at current size — duplicate customer/material records become a real operational cost as headcount and data volume grow, but may not be causing measurable pain today. Worth revisiting this one specifically if/when the business crosses a size threshold, rather than deciding it purely on current-state evidence.

**Decision:** _______________

---

## Domain 13 — Integration, APIs & Enterprise Connectivity

**Canon (§10.4–§10.7):** A formal API/contract governance layer — API/Service/Event/Command/Query Contracts, Consumer/Provider Registrations, Version Definitions, Compatibility Rules — each with its own lifecycle.

**What exists in frozen:** Every top-level function in every file is directly exposed as a `google.script.run` RPC target with no intervening route, contract, or version-registry layer of any kind — "the API" is the full set of exposed function signatures, governed only informally by not renaming or removing them.

**What exists in Standalone — genuinely more than frozen, still short of Canon's scope:** A real, narrow `packages/contracts` package (`@gifthatkeos/contracts`) with typed HTTP-boundary shapes (health/error/database-health responses), explicit in its own scope comment that it covers transport/error/health only, not the full business-domain surface. This is a real, previously-uncredited improvement over frozen found this session — worth knowing when deciding this one, since "genuinely more than frozen has" changes the calculus slightly from every other item in this brief.

**The gap:** No consumer/provider registration, no formal versioning, no compatibility-rule mechanism, and the typed-contract coverage that does exist is limited to a handful of transport-level shapes, not the full API surface.

**Consideration:** Relevant mainly if/when Standalone grows external integration partners (a mobile app, a third-party marketplace connector, a partner API) — internally, Standalone's own typed request/response interfaces across services already provide some of what formal contracts would, informally. Lower urgency than most items here for a single-tenant internal ERP.

**Decision:** _______________

---

## Domain 20 — Sales, Commercial Operations & Revenue Management

**Canon (§10.5–§10.6):** Formal Quotations (with their own 6-state lifecycle), Price Books, Sales Territories, Contracts, and Commission Plans.

**What exists:** One naming artifact — frozen's CRM stage-compatibility map aliases a legacy external stage name `"Quotation"` onto its own `"Payment Requested"` stage. Not an actual Quotation document or entity with its own lifecycle. No Price Book, Commission Plan, Sales Territory, or formal Contract entity exists anywhere. GiftHatke's actual sales motion runs directly through CRM leads converting to Orders, with pricing embedded per-order.

**The gap:** The entire commercial-governance layer Canon names — separate from the order-taking mechanics already reviewed in Domain 1 — doesn't exist.

**Consideration:** Whether this matters depends on whether GiftHatke's pricing is genuinely ad hoc per order today (in which case a Price Book/Commission Plan layer would be new, real capability) or whether informal consistency already exists (a shared pricing spreadsheet, standard rate cards) that just isn't modeled as its own governed entity. The Domain 23 item below (Reseller/Partner Dashboard) touches a closely related margin/pricing concept — worth deciding these two together if that wave gets scoped.

**Decision:** _______________

---

## Domain 22 — Marketing, Campaign Management & Customer Engagement

**Canon (§10.5–§10.6):** A full Campaign/Audience-Segment/Promotion/Marketing-Experiment model with its own 9-state lifecycle.

**What exists:** Free-text `Source` and `Campaign` fields on each CRM Lead — attribution data, not a Campaign entity with its own definition, execution, or measurement lifecycle. No Audience Segment, Promotion, or Marketing Experiment entity exists.

**The gap:** The entire domain, as its own governed thing, is absent — what exists is a field-level echo of the concept (knowing *which* campaign a lead came from) with none of the surrounding governance (defining, running, or measuring a campaign as its own object).

**Consideration:** Likely the lowest-friction "formally descope" candidate in this brief if marketing campaigns aren't currently run through any system at all (i.e., attribution is captured but campaigns themselves live in ad platforms, not GiftHatke's own tooling) — worth a quick factual check before deciding rather than assuming either way.

**Decision:** _______________

---

## Domain 23 — Partner, Supplier & Ecosystem Relationship Management (with an important scope clarification)

**Canon (§10.2–§10.7):** *Upstream* relationship management — GiftHatke qualifying, evaluating, and governing its own suppliers and vendors (Partner Master Management, Supplier Governance, Partner Capability/Risk Management, an 11-state lifecycle from Identified through Strategic to Retired/Archived). Contains no mention of resale, storefronts, wallets, margin tiers, or pricing engines.

**What exists:** No partner/vendor-relationship system of any kind — only a free-text `Default Supplier` field on Materials (already noted under Domain 2).

**Important clarification, not a defect, surfaced this session:** the vault's own separately-planned **Retailer/Reseller/Partner Dashboard** wave — free registration, a Partner Wallet, SKU-specific pricing, target margins, price-floor guards, publication statuses — is about *downstream* channel partners who buy from GiftHatke and resell. That is a materially different concept from this Canon domain's upstream supplier-governance content, and **is not itself one of the 44 certified Canon domains** as currently scoped. If that Reseller wave proceeds, it needs to be either mapped onto this domain as the nearest conceptual fit (a downstream mirror of upstream governance) or treated as its own addition — Canon doesn't already specify it in the detail the vault's own Titan Lock planning notes assume.

**Consideration:** Two genuinely separate decisions live under one domain number here. (1) Does GiftHatke need formal upstream supplier governance (qualification, risk scoring, lifecycle tracking) — likely a "formally descope" candidate for a business this size unless supplier risk has caused a real incident. (2) Is the downstream Reseller/Partner Dashboard wave proceeding, and if so, how does it relate to Canon's 44 domains at all — a scoping question for whoever owns that wave, separate from this brief.

**Decision (upstream supplier governance):** _______________
**Decision (Reseller/Partner Dashboard wave relationship to Canon):** _______________

---

## Narrower items worth knowing about, not given their own section

Two findings are real absences but sit *inside* an otherwise well-implemented domain, not a whole missing domain — different enough in shape that they don't fit this brief's per-domain format cleanly:

- **Domain 1/5 — Attachments and reverse logistics.** Order Attachments were closed this session (PHB-3, now real and live). Shipping's equivalent — Return Requests, Return Shipments, Reverse Logistics Cases as their own governed entities (Canon §10.5/§10.7) — remains absent in both systems; returns exist only as two status values (`RTO Initiated`, `Returned`) plus two free-text fields on the Shipment row itself.
- **Domain 6 — Journal Entries, Journal Lines, General Ledger Accounts.** Canon certifies formal double-entry bookkeeping structures; neither system has them — what exists instead is a flat, single-entry transaction log (`Finance_Transactions`) that can still produce correct totals if consistently applied, but doesn't debit one account and credit another for a single business event. Given Finance's external-scrutiny exposure (tax, lenders, auditors), this is worth deciding alongside Domain 6's lifecycle-vocabulary item in the companion brief, not in isolation.

If either of these is worth its own decision line, say so and it'll get one; they're recorded here rather than omitted so nothing gets lost.

---

## Decision authority

Same standing as the companion brief and the recovery procedure: **Hitendra Chug**, sole operator, no delegation or secondary approver.

## After a decision is recorded

Same mechanism as the companion brief: decisions get reflected as closing notes in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` and the Standalone parity matrix, matching established voice. Path (B) decisions close with no code change — just a recorded, explicit "not building this" that stops it from being silently re-discovered as an open question in some future pass. Path (A) decisions open new, separately-scoped implementation work. Path (C) decisions close with a recorded rationale for why the lighter substitute is sufficient.

Between this brief and the companion vocabulary brief, every remaining category of GAP-001 blocker this report has identified across the full 44-domain sweep is now on record somewhere with a decision requested. Deciding everything in both briefs would close GAP-001's remaining content; deciding nothing leaves it exactly where it stands today — open, blocking, and now fully and precisely enumerated for the first time.
