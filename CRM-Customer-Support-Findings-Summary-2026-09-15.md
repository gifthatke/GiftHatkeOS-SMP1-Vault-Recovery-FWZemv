# CRM & Customer Support — What the Audit Found

**Date:** 2026-09-15
**Covers:** the CRM (lead/enquiry management) module and Customer Support as a Canon-named business capability, as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 19 and Domain 32 sections), if anyone wants to check the underlying evidence.

## The short version

CRM (how you track a customer enquiry from first contact through to a paid order) carries over faithfully from the old system to the new one, including one real but rare defect that exists in *both* systems, not something the migration introduced. Separately, a formal "Customer Support" capability — support tickets, service-level tracking, escalation — was never built in the old system in the first place, so there's nothing for the new system to be missing. Below explains both.

## 1. A rare but real risk: a failed conversion can create a duplicate order

When a lead gets marked "Order Created," two things happen: the order gets created, and then the lead gets marked converted, as two separate steps with nothing tying them together. If the first step succeeds but the second one fails — a real possibility, just an uncommon one — the lead is left looking unconverted. If someone (or something) retries the conversion at that point, it creates a **second, duplicate order** from the same lead, with nothing stopping it.

This is not something the new system introduced. The old system has exactly the same two-step, unprotected sequence, and the new system's version has an explicit note in its code confirming this was a deliberate choice to match the old system's behavior, not an oversight. So this is a pre-existing weak spot carried forward faithfully, not a new bug.

**Why it's not urgent**: it only shows up when the second step happens to fail at exactly the wrong moment — not something that happens in the ordinary course of business. But it's flagged as worth fixing before or shortly after handover, because if it does happen, the result (a real duplicate order reaching production and shipping) is the most customer-visible problem found in this whole audit. A proven fix pattern already exists elsewhere in the new system (the same shape of problem was solved correctly for a different module), so a correction here — if you want one — would be a known, low-risk pattern, not something to invent from scratch. No scope document has been written for this fix yet; that would be a next step if you want to authorize it.

## 2. CRM doesn't check permissions action-by-action — same as several other older modules, not a new weakness

The old system's CRM module only checks whether someone has access to open the CRM screen at all — it doesn't separately check permission for each individual action inside it (editing a lead, changing its stage, marking it lost). The new system currently has the same coarse-grained gap for this specific module.

This sounds worse than it is in context: CRM is one of several **older** parts of the original system (alongside Orders, Inventory, and Production) that were all built this way. The **newer** parts of the system (Settings, Company, Roles, Finance) do have real, per-action permission checks, and the new system correctly matches that split — stricter where the old system was stricter, and no stricter than the old system where it wasn't. So this isn't the new system falling behind; it's the new system faithfully carrying forward a security-hardening gap that already existed. Whether it's worth tightening (in both systems, or just going forward in the new one) is a judgment call about acceptable risk for your team size, not something this audit is recommending either way.

## 3. There is no dedicated "Customer Support" system in either the old or new system

This is worth stating plainly because the original business blueprint document does name Customer Support (support tickets/cases, service-level agreements, escalation handling, omnichannel support) as a capability the business should eventually have. **Neither the old system nor the new one actually built it.** What exists today, in both systems, is CRM (leads), the customer approval workflow (below), and general order-status tracking — real things, but none of them a ticketing/case-management system.

**What this means for you**: if your team currently handles customer support through some other channel entirely (email, WhatsApp, a spreadsheet, phone), that's expected — there was never a dedicated in-system tool for it, so nothing was lost in the migration. If you'd eventually want a real support-ticket system inside GiftHatkeOS, that would be new capability to build from scratch, not a parity gap to close — a different kind of project than everything else in this audit, and not something recommended here one way or the other.

## 4. A genuine bright spot: customer artwork/personalization approvals carry over cleanly

Separate from CRM itself, the workflow where a customer's personalization/artwork needs their sign-off before production continues — checked directly, and it's a clean, faithful match between the old and new systems. No concerns here.

**One related item already scoped elsewhere, mentioned for completeness**: the old system also computes real customer-relationship analytics — which leads are stuck without follow-up, how much a repeat customer is worth over time, whether too much revenue depends on too few customers. The new system doesn't compute these yet, though the plumbing to read the underlying data already exists. This was already covered in this audit's broader Business Intelligence findings (a separate document, if useful) — not repeated in full detail here since it's not specific to CRM/Support.

## What this doesn't affect

- CRM's existing certified/closed status is unchanged. This isn't a reopening of that closure — it's additional scope information.
- Nothing here has been implemented, fixed, or changed. This is a findings summary, not an action.
- Neither finding above is specific to the new system being risky or behind — both are cases of the new system faithfully matching the old one, including its existing weak spots.

## Bottom line

One real, rare, fixable defect (the duplicate-order risk) that exists in both systems and is worth fixing before or shortly after handover, with a known fix pattern already proven elsewhere — not yet scoped in detail. One security-hardening question (per-action permission checks in CRM) that's a judgment call, not a finding that demands action. And one thing to simply be aware of: there's no dedicated customer-support ticketing capability in either system, which is expected rather than a gap, unless you want to scope building one as new work.
