# Production & Shipping — What the Audit Found

**Date:** 2026-09-15
**Covers:** the Production (manufacturing job tracking) and Shipping & Fulfillment modules, plus two adjacent Canon-named capabilities (Manufacturing Excellence, Supply Chain Planning), as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 2, 3, 5, 33, and 34 sections), if anyone wants to check the underlying evidence.

## The short version

Production carries over cleanly overall — the job stages, quality-check gate, and field-level detail all match exactly. One real defect exists, but it's shared identically by both systems (not something the migration introduced) and sits at the Inventory/Production boundary, not inside Production itself. Shipping also carries over cleanly, with one open question worth a decision: the new system is *stricter* than the old one in a specific way, and nobody has confirmed yet whether that was deliberate. Two related capabilities named in the original blueprint — formal manufacturing-performance tracking and supply/demand planning — were never actually built in either system, so there's nothing there to be missing.

## 1. Production itself: clean, verified match

Checked directly, field by field: the 9 production job stages, the 4 quality-check outcomes, the 7-item quality checklist, and the rule that a job can't be marked "Completed" without first passing QC all match exactly between the old and new systems. No gaps found in Production's own core tracking.

Same as CRM (covered in the last summary): Production doesn't check permissions action-by-action, only at the module level — but this is the same older-generation pattern shared with Orders, Inventory, and CRM, faithfully carried forward rather than a new weakness introduced by the migration.

## 2. One real, shared defect: a failed multi-material job can leave some materials silently un-deducted

When a production job needs several different materials, the system deducts each one from inventory as a separate step, one at a time. If that process fails partway through — after some materials were deducted but not all — the system's own check for "has this already been done" only looks at whether *any* deduction happened, not whether *all* of them did. That means a partial failure gets treated as "already handled," and a retry silently skips the materials that were never actually deducted, with no automatic way to finish the job the normal way.

**This is identical in both systems** — the new system's code has an explicit comment confirming it was built this way on purpose, to match the old system exactly, not because anyone missed it. A proven fix already exists elsewhere in the new system for this exact shape of problem (goods-receipt posting in Procurement solves it correctly, using a safeguard the old system's technology couldn't offer), so if you want this fixed, it's a known pattern to apply, not something to invent. Not yet scoped in detail — that would be a next step if you want to authorize it. Lower urgency than the CRM duplicate-order risk sent earlier, since the consequence here is an inventory-accounting discrepancy, not a customer-facing duplicate.

## 3. Shipping: a genuine improvement, but nobody has confirmed it was meant to be one

When a shipment's status changes, the system also tries to update the linked order's status to match. In the old system, if that second part fails, the shipment update still goes through and nobody sees an error — the order and shipment can silently drift out of sync forever, with no record that anything went wrong. The new system doesn't have that silent-failure behavior: if the order-sync step fails, the *entire* shipment update fails and the employee doing it sees an error immediately.

This is arguably better behavior — failing loudly instead of drifting silently is usually the safer choice — but it's a real behavior change from the old system, and there's no note anywhere confirming it was a deliberate design decision rather than something that happened by accident while building the new system.

**What I need from you (or whoever can speak to this)**: was this stricter behavior in the new system an intentional choice? If yes, nothing needs to change — it just needs to be written down as a confirmed decision. If it turns out to have been accidental, the recommendation would still be to keep it as-is (failing loudly beats silent data drift), which would make this a documentation task rather than a code change either way. This is the only item in this pair of modules that's genuinely waiting on a decision rather than a build.

## 4. Two blueprint capabilities that were never built in either system

The original business blueprint names two things that sound related to Production and Shipping but turned out not to exist anywhere, in either the old system or the new one:

- **Formal manufacturing-performance tracking** (the kind of detailed equipment-efficiency, downtime-cause, and standardized-work tracking a larger factory floor might use) — checked directly, no trace of it in either system.
- **Supply and demand planning** (forecasting what you'll need to produce or stock before you need it, capacity planning, safety-stock calculations) — also checked directly, not present in either system. The old system's "planning" module that does exist is actually part of Finance (budget planning, already covered in the Finance summary), not this.

**What this means for you**: nothing was lost migrating, since neither capability ever existed to lose. If either would genuinely help the business at your current scale, that's new capability to consider building — a different kind of decision than anything else in this audit, not a parity gap to close.

## What this doesn't affect

- Production's and Shipping's existing certified/closed status is unchanged. This isn't a reopening of either closure — it's additional scope information.
- Nothing here has been implemented, fixed, or changed. This is a findings summary, not an action.
- The BOM material-deduction issue (item 2) is not specific to the new system being risky — it's the new system faithfully matching an existing weak spot in the old one.

## Bottom line

One real, shared defect (partial material deduction on job failure) that's lower urgency and not yet scoped in detail, with a known fix pattern already proven elsewhere if you want it addressed. One genuine open question about Shipping's stricter error-handling that just needs someone to confirm it was intentional — the cheapest item in this whole audit to resolve, since it may turn out to need no code change at all, just a decision on record. And two named-but-never-built capabilities (manufacturing-performance tracking, supply/demand planning) that are expected absences, not gaps, unless you want to scope them as new work.
