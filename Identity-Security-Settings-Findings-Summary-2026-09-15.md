# Identity, Security & Settings — What the Audit Found

**Date:** 2026-09-15
**Covers:** user roles/permissions, company & organization setup, staff-to-role assignment, system audit trails, and the various configuration ("Settings") modules — Tax, Marketplace channels, document numbering — as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 8, 9, 14, 25, 39, 40, and 41 sections, plus the ERP84/85/86 Settings passes), if anyone wants to check the underlying evidence.

## The short version

This is the most reassuring summary sent so far. There's no real gap in this area — the two things worth mentioning are both cases of the **new system doing something better than the old one**, not falling short of it. Company/organization setup, tax handling, and sales-channel configuration all match cleanly. There's one small, genuinely harmless oddity, and one operational note for whoever manages the technical side of hosting, unrelated to how the business itself runs.

## 1. Why several earlier summaries mentioned "no per-action permission checks" — and why that's not a blanket weakness

Earlier summaries (CRM, Production, Inventory) each noted that those modules only check whether someone can open the screen at all, not each individual action inside it. This is the module where that pattern was actually traced to its source, so it's worth explaining properly here: it's **not** true across the whole system. The old system was built in two generations — an older one (Orders, CRM, Production, Inventory, Shipping) that only has module-level access checks, and a newer one (Roles, User Management, Settings, Company, Organization, Finance) that does check permissions properly, action by action. The new system correctly matches this same split rather than blurring it — stricter where the old system was stricter, and no weaker anywhere the old system wasn't. So this isn't one recurring weakness; it's an accurate carry-over of an intentional (if uneven) design from the original build.

## 2. Staff-to-role assignment: the new system is actually cleaner than the old one

Checked directly: the old system has **two separate, competing versions** of the feature that assigns staff members to roles — built at different times, saving to the same spreadsheet but with different layouts, and (this is the concerning part) both using some of the exact same internal function names. Because of how the old system's technology loads its files, whichever version happens to load last silently wins for a few of those shared functions — not something anyone would notice without reading the source directly. On top of that, neither version's screen is actually reachable through the old system's normal menus today.

**The new system has one single, clean version** of this feature, properly connected to its own screen and menu, matching the more complete of the old system's two competing versions (including one capability — restoring a revoked assignment — that only one of the old versions had). No gap here — if anything, the new system fixed a real, if invisible, mess in the old one. Nothing needs fixing; this is included so you know it was checked.

## 3. Who-did-what tracking: also better in the new system

The old system has a basic technical activity log, but it's opt-in (only logs when a developer specifically chose to record something) and capped at the last 200 entries. The new system has something more thorough: **every single permission check across the entire application** — every time anyone tries to do anything that requires access rights, whether it succeeds or is denied — gets permanently recorded with who did it, what they tried to do, and the outcome. This wasn't something anyone had to specially build for this purpose; it's a byproduct of how the new system checks permissions in general, and it turned out to be more complete than the old system's dedicated logging feature. Another case of the new system being ahead, not behind.

## 4. Company, branch, and department setup: matches cleanly

Company details (legal name, GSTIN, PAN, registered address, bank accounts) and organizational structure (branches, departments, cost centers) both carry over correctly. Neither system supports running multiple separate companies under one login (with transactions between them) — but that was never expected to, since the business currently operates as one company. Nothing to act on here.

## 5. Tax handling and sales-channel setup: both confirmed correct

Two configuration areas were checked line-by-line against the old system's exact calculations: GST/tax handling (already mentioned in the Finance summary — confirmed correct) and how external sales channels (Amazon, Flipkart, Meesho, and similar) are configured — connection settings, order-routing rules, fulfilment rules. Both match the old system's logic closely. No concerns in either.

## 6. A small, genuinely harmless oddity: an unused numbering system

Both the old and new systems have a fully-built feature for generating sequential document numbers (order numbers, invoice numbers, and similar) in a controlled, configurable way. Neither system actually uses it — both generate their real order and customer numbers through a separate, simpler mechanism instead, and always have. This isn't a migration issue, since it's identical in both systems; it's just worth knowing that this particular feature exists but has never actually been switched on, in case anyone goes looking for it later expecting it to be in use.

## 7. One item for whoever manages hosting/infrastructure, not a business-process finding

The new system's basic "is everything running" check currently always reports "OK" regardless of whether it actually is — it doesn't yet check the real database connection the way it could. The new system does have more sophisticated health-checking built underneath, but it's deliberately not switched on for outside access yet, based on a note left directly in the code. This may well be entirely intentional for where the project is right now. It's noted here only so whoever is responsible for making sure the live system is actually being watched properly knows to follow up — it's an operations/hosting question, not something that affects how your team would use the software day to day.

## What this doesn't affect

- Nothing here reopens any existing certified/closed status. This is additional scope information.
- Nothing here has been implemented, fixed, or changed — though for items 2 and 3, there's nothing to fix, since the new system already does the better thing.
- None of this area required a "how are we going to build this" plan the way Finance, the Dashboard, or the analytics work did — there's no missing capability here to scope.

## Bottom line

The cleanest area of the whole audit: two genuine improvements (a cleaner staff-assignment system, and a more thorough who-did-what record), company/tax/sales-channel setup all confirmed correct, one harmless unused feature worth knowing about, and one small note for whoever handles hosting. Nothing here needs a decision from you.
