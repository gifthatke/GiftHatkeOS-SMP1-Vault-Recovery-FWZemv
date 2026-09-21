# Inventory & Procurement — What the Audit Found

**Date:** 2026-09-15
**Covers:** the Inventory (materials/stock) and Procurement (purchase requisitions, purchase orders, goods receipt) modules, as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 2 sections — this got more scrutiny than almost any other module in the audit, seven separate passes), if anyone wants to check the underlying evidence.

## The short version

This is the strongest news of any module summary sent so far. Inventory and Procurement's core data (material records, purchase order lifecycle) match the old system exactly. And in two specific places, the new system is genuinely **safer** than the old one — not just matching it, improving on it — because it uses database technology the old system's spreadsheet foundation never had access to. There's one real shared weak spot (already covered in the Production/Shipping summary, recapped briefly below) and one old-system-only oddity that isn't actually the new system's problem at all.

## 1. Two genuine improvements: purchase orders and goods receipt are safer in the new system

Both of these involve a "do two related things, one after the other" pattern — create a purchase order from a requisition and mark the requisition as converted; or post a goods receipt while updating inventory and the purchase order together. In the old system, both are done as two separate, unprotected steps: if the second step fails after the first succeeds, you can end up with inconsistent records (a duplicate purchase order on retry, in one case; inventory and purchase-order changes with no matching goods-receipt record explaining them, in the other — arguably the hardest kind of problem to notice, since it leaves no obviously-wrong record, just a silently incomplete one).

**The new system does not have either exposure.** Both operations are wrapped as one all-or-nothing database transaction — checked directly, not assumed — so a failure partway through undoes the whole thing cleanly instead of leaving a partial mess. This is a real, verified improvement over the old system, using a safeguard the old spreadsheet-based system structurally couldn't offer. Nothing needs fixing here; this is included so you know it was checked and the news is good.

## 2. The "two requisition sheets" mystery — solved, and it's not your new system's problem

Earlier in this audit, something looked odd: purchase requisition data appeared to exist in two different places in the old system. That's now explained: at some point, a one-time migration moved requisition data from an old spreadsheet location to a newer one, but a separate part of the old system — the low-stock reorder-alert widget on the Inventory screen — was never updated to look at the new location. So today, anyone looking at that specific widget in the old system sees data frozen at the moment of that migration, silently disconnected from anything that's happened in Procurement since.

**This is entirely a quirk of the old system, not something to fix in the new one.** The new system's Inventory and Procurement code doesn't have the old system's split-location structure that caused this in the first place, so it doesn't inherit the problem. Recorded here only for completeness — no action needed on your end.

## 3. Same permission-check pattern as the other older modules

Same finding as CRM and Production (covered in earlier summaries): Inventory and Procurement don't check permissions action-by-action, only at the module level — the same older-generation pattern shared across Orders, CRM, and Production, faithfully carried forward rather than a new weakness. Not repeated in full detail here since it's the same story each time.

## 4. One shared weak spot, already covered — recapped briefly

The multi-material production-job deduction issue sent in the Production & Shipping summary sits right at the Inventory/Procurement boundary: if a job needing several materials fails partway through deducting them, a retry can silently skip whatever wasn't deducted the first time. It's identical in both systems and not urgent. See that summary for the full explanation — not repeating it here to avoid sending the same thing twice.

## 5. Material records themselves: clean, exact match

Checked field by field: how individual materials/stock items are structured and tracked matches exactly between the old and new systems. No gap found here.

## 6. One thing not yet built, mentioned for completeness

The old system can compute real inventory analytics — which items are running low against actual production demand, which suppliers you're most dependent on, a suggested reorder plan, how many days of stock cover you have left. The new system doesn't compute these yet, though the underlying data it would need already exists and is readable. This was already covered in this audit's broader Business Intelligence findings (a separate document, if useful) — not specific enough to Inventory/Procurement alone to repeat in full here.

## What this doesn't affect

- Inventory's and Procurement's existing certified/closed status is unchanged. This isn't a reopening of either closure — it's additional scope information.
- Nothing here has been implemented, fixed, or changed. This is a findings summary, not an action.
- Item 2 (the reorder-queue quirk) is specifically **not** a new-system issue — flagged for completeness, not for action on your part.

## Bottom line

The best news of any module covered so far: two places where the new system is verifiably safer than the old one, core material data matching exactly, and the one old-system oddity turning out not to be your new system's problem at all. The only real open item for this pair of modules is the shared production-job material-deduction issue, already sent in the Production & Shipping summary — nothing new to act on here beyond that.
