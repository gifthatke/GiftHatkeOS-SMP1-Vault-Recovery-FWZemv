# Finance — What the Audit Found

**Date:** 2026-09-15 (updated — see note below)
**Covers:** the Finance & Accounting module, as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you (or your accountant/bookkeeper) what's needed to decide whether anything requires action.

**Update note:** this replaces the earlier versions sent today. Two things changed since the last version: (1) the new system has **not** been handed over to your team yet, so anything below is a pre-handover gap to plan for, not a live-system question needing an urgent answer — an earlier draft was written before that was confirmed and wrongly implied otherwise; (2) the two gaps below now both have a detailed, ready-to-review implementation plan, so this is no longer just "here's a problem," it's "here's a problem and here's exactly what it would take to fix it."

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 6 sections) and `Finance-Write-Workflows-Pre-Handover-Scope-2026-09-15.md` (the implementation plan for items 1 and 2 below), if anyone wants to check the underlying evidence.

## The short version

Finance is already certified on the new system for what it was tested against. This audit looked at something narrower and more specific: whether the new system can actually *do* the things your bookkeeper does every day — record a payment, record an expense, plan a budget. Two real gaps were found. Neither is a live-system emergency, since the new system isn't handling real operations yet. Both now have a precise, ready-to-hand-off implementation plan, so the open question is really just "when do we want this built," not "what's wrong."

## 1. The new system has no way to record a payment or an expense yet

This is the more important of the two, and it's a missing *action*, not a missing report.

The old system has a real, careful process for recording that a customer paid you: it checks the order isn't cancelled, checks the payment doesn't exceed what's owed, checks you're not recording the same payment reference twice, writes a proper transaction record, and then updates the order's paid amount and status — all as one protected step. The same exists for recording expenses and for reversing a mistaken entry.

The new system's Finance section, as it stands today, only has code to *display* things (the workspace view, the timeline view) — there is no code anywhere to record a receipt, record an expense, or reverse a transaction. The only way an order's "paid" amount could currently change in the new system is if someone directly edited that field the same way they'd edit any other order detail — skipping every one of the old system's checks.

**The good news, found while scoping this**: it's a narrower gap than it first looked. The new system's underlying data structures and database layer for handling transactions, expenses, and cashbook entries are already fully built — what's missing is the business-logic layer on top (the validation and sequencing rules) and the on-screen actions to trigger it. A full, step-by-step implementation plan for both Record Receipt and Record Expense now exists, including a recommended order to build them in (Record Receipt first, since its foundation is furthest along).

**What this means for you**: before your team starts using the new system for real orders, this needs to be built and tested — the same way every other piece of this system went through its own build-test-approve cycle. It is not something to work around informally once live; it's something to finish before going live. No decision is needed from you today beyond knowing this is on the list.

## 2. Budget planning exists in the old system and doesn't exist in the new one

The old (Apps Script) system has a working budget module: you can set a planned amount per department/cost-center/month, it tracks a revised amount and an approved amount, and it automatically recalculates the actual amount and the variance against your real transactions.

The new (Standalone) system does not have this yet. Everywhere "budget" appears in its code traces back to one thing: a list of *budget category names* — essentially a dropdown list — not actual budget entries with amounts, months, or variance tracking. A full implementation plan for this now exists too, alongside item 1's.

**One open design question, not yet answered**: the old system actually has two different things called "Cost Center" — a Finance-planning one and a separate organizational one — that were never reconciled with each other. Whoever eventually scopes the real build will need to decide how (or whether) to merge these, which is a design call, not something this audit can resolve on its own.

**What I need from you**: do you (or whoever handles budgeting) actually use the old system's budget-vs-actual feature today? If yes, this is worth scheduling alongside item 1. If no, it can reasonably wait — it's a planning/analysis tool, not something that blocks recording a payment or processing an order.

## 3. Two genuine bright spots, confirmed while looking at all this

- **GST/tax records are solid.** The GST handling in the old system is genuinely well-built — full CGST/SGST/IGST/CESS breakdown, interstate detection, place of supply, HSN codes, the works — and a direct line-by-line check confirmed the new system reproduces the same calculations correctly. No concerns here.
- **Finance's analytics are already fully built in the new system**, not just planned: profitability by product/channel/customer, expense concentration, receivables aging, and profit-leakage detection all exist today and work correctly, reachable from the Reports section. This is a real, complete capability, not a placeholder — worth knowing, since most of what's listed above is about gaps.

## 4. The underlying money records aren't classic double-entry bookkeeping

This is a structural fact, not a bug, and it's true of both the old and new systems equally — so it's not something the migration introduced.

Both systems record a transaction as one row against one account (e.g., "₹5,000 Customer Receipt into the Cash account"), rather than the formal "debit one account, credit another, every entry balances" style a chartered accountant or auditor would usually expect from a general ledger. There's no Journal Entry / Chart of Accounts structure underneath — what exists instead is a simpler transaction log plus separate receivables, payables, and expense sheets.

To be clear about what this does and doesn't mean:
- It does **not** mean any figure you're seeing is incorrect. A well-run transaction log can still produce correct totals.
- It **does** mean that if you ever need to hand this off to an external auditor, a bank, or a new CA who expects to see a standard general ledger / trial balance structure, the underlying data won't be shaped that way.

Whether that matters depends entirely on your actual compliance/audit obligations at your current scale — that's a call for you and your CA, not something to scope until that question is answered.

## What this doesn't affect

- Finance's existing certified/closed status is unchanged. This isn't a reopening of that closure — it's additional scope information about what that closure covered and didn't.
- Nothing here has been implemented, fixed, or changed yet. This is a findings-and-planning summary, not an action.
- The one Finance-adjacent item that already got fixed this audit (a data-integrity bug in how Orders and Customers interact) is unrelated to the items above and is already live.

## Bottom line

Two real capability gaps (recording payments/expenses, and budget planning), both now fully scoped and ready for a build decision whenever you want to authorize it — neither is urgent in the "something's wrong right now" sense, since the new system isn't handling live operations yet. One open design question (the two "Cost Center" concepts) needs a decision before the budget piece can be built. Two genuine strengths (GST handling, Finance analytics) are worth knowing about too, not just the gaps.
