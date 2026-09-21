# Finance's Structural Accounting Model — A Decision Note, Not a Scope Document

**Date:** 2026-09-16
**Status:** DECISION-SUPPORT NOTE ONLY — deliberately not an implementation scope. Consolidated plan item 10 explicitly says this item "should not be scoped until the underlying business question... is answered — scoping an implementation before that question is answered risks building the wrong thing." This document exists to make that question answerable, not to answer it.
**Origin:** consolidated plan Tier 2 item 10

## The actual question

**Does GiftHatke need formal double-entry bookkeeping — a Chart of Accounts, Journal Entries, and a General Ledger — or does the current transaction-log model already meet its real accounting and compliance obligations?**

This is a business and accounting question, not an engineering one. It depends on facts about GiftHatke specifically — its transaction volume, whether an external accountant or auditor already relies on a GL export from somewhere else, whether it has ever been asked for one by a bank, investor, or tax authority, and what its statutory filing obligations actually require at its current scale. None of that is visible from source code, and this document does not attempt to answer it.

## What exists today, in both systems, confirmed by direct read

Neither frozen nor Standalone implements formal double-entry bookkeeping. This is identical in both — not something the migration changed, and not evidence that any current figure is wrong. What both systems have instead:

- **Transactions** (`FinanceTransaction`): one row per money-moving event (a receipt, an expense, a payment), each tagged with a type, an amount, a payment method, and a reference back to whatever it relates to (an order, a supplier). This is a log, not a ledger — there is no debit/credit pair, no notion of which accounts move together.
- **Receivables / Payables**: separate, purpose-built lists of outstanding amounts owed to or by GiftHatke, each tied to a customer or supplier.
- **Cashbook**: a running-balance ledger of cash movements in and out of specific accounts (`FinanceCashbookEntry`, which does carry a `runningBalance` — the closest thing either system has to ledger-style bookkeeping, but scoped to cash only, not a general chart).
- **Accounts** (`FinanceAccount`): a bare list of bank/cash accounts, each just an ID and an opening balance — not a Chart of Accounts. There is no Asset/Liability/Equity/Revenue/Expense classification, no account hierarchy, no normal-balance-side concept anywhere in either codebase.
- **GST Register**: transaction-level tax entries (output/input GST), already used for statutory GST liability reporting — the one place either system already does something resembling formal tax-ledger bookkeeping, narrowly scoped to GST.

Every profit/loss and balance figure Standalone currently reports (`GET /reports/financial`, the Executive Dashboard's KPI cards, `executiveFinanceIntelligence`) is computed by summing and netting these transaction-log rows directly — not by rolling up ledger account balances. This works and is arithmetically sound for what it computes; it just isn't a general ledger.

## What "yes, build a general ledger" would mean, in shape only — not a plan

If GiftHatke's business/compliance needs genuinely require formal double-entry bookkeeping, the shape of that work — described here only so the decision's weight is clear, not as anything resembling an implementation plan — would include: a Chart of Accounts as a real, owned entity (not just a bank-account list); Journal Entries as the atomic unit of every financial event, replacing today's single-row transaction log; a posting rule for every money-moving action across Finance, Orders, Procurement, and anywhere else money or inventory value changes hands, so each event produces a balanced debit/credit pair rather than one row; and a way to roll journal entries up into trial balances and the standard financial statements. This is a foundational restructuring of the Finance domain, not an additive feature — closer in scale to a second Finance module than a Finance enhancement, and every other module that currently posts a `FinanceTransaction` (Orders, Procurement, Shipping cost postings) would need to change what it does when money moves.

## What "no, the current model is sufficient" would mean

Nothing changes. The current transaction-log-plus-receivables/payables model keeps computing the same figures the same way it does today, in both frozen and Standalone. This item closes with no code change — the same resolution shape as item 9 above, arrived at for a different reason (there, both branches of an unknowable question converged on one answer; here, the answer genuinely depends on external facts, and "no GL needed" is a fully legitimate answer, not a default being applied to avoid the work).

## Questions worth asking to actually decide this

- Has GiftHatke's accountant, auditor, or tax filing process ever asked for a trial balance, a general ledger export, or anything resembling formal double-entry records — or have they always worked from the transaction-log/receivables-payables figures as-is?
- Is there a specific compliance trigger on the horizon (a funding round, a loan application, a statutory audit threshold GiftHatke is approaching) that would require GL-level records where none were needed before?
- Does GiftHatke's current bookkeeping practice (whatever happens outside this software, if anything, in a separate accounting tool) already produce formal ledger records independently — meaning this software's simpler model is deliberately not the system of record for that purpose?

## What this document deliberately does not do

It does not recommend building a general ledger. It does not recommend leaving the current model as-is. It does not scope, estimate, or plan an implementation for either answer. It exists only so that whoever can answer the actual business question — likely GiftHatke's ownership, together with whoever handles its accounting today — has a clear, accurate picture of what exists, what a "yes" would actually mean in scale, and what to ask before deciding. Once that question is answered, either outcome closes this item: "no" closes it exactly as it is; "yes" turns into its own, separately-authorized scope document, written only after the decision, not before.
