# Order Attachments, Order Notes & the Task Board — What the Audit Found

**Date:** 2026-09-15
**Covers:** three everyday features from the old system that don't exist yet in the new one — attaching files to an order, keeping a running log of notes on an order, and the daily task-tracking board every employee sees on login
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 1 and 11 sections), and each item has its own full implementation plan: `Order-Attachments-Pre-Handover-Scope-2026-09-15.md`, `Order-Notes-Pre-Handover-Scope-2026-09-15.md`, and `Task-Work-Board-Pre-Handover-Scope-2026-09-15.md`.

## The short version

These three are grouped together because they're all the same kind of finding: real, everyday features your team currently uses in the old system that simply haven't been built yet in the new one — not bugs, not design disagreements, just work that hasn't happened yet. Two of the three (Attachments and the Task Board) are the kind of thing employees would notice missing on their very first day. All three now have a complete, ready-to-build implementation plan.

## 1. Order Attachments — attaching a file to an order

In the old system, anyone can attach a reference file to an order — a customer-supplied photo, a signed document, a design reference — with a category and short description, and it shows up in the order's history. This is a routine, everyday action for an order-heavy business like yours.

**The new system has none of this** — no way to attach anything to an order at all, confirmed by a thorough search of the code. This is a build-from-scratch item, not a case of finishing something partially started. A full implementation plan exists. Worth prioritizing, since this is the kind of thing that would be noticed on day one, not an edge case.

One small thing worth knowing: even the old system never had a way to *remove* an attachment once added, only to add new ones — so this isn't something the new system needs to do differently than the old one, just needs to actually exist.

## 2. Order Notes — a running, dated log of comments on an order (different from the notes box you already have)

This one needs a bit of untangling, because there are genuinely two different things both called "notes," and it's worth being precise about which one is missing.

The new system already has a simple notes box on each order — a text field anyone can type into and overwrite, the same way you'd edit the delivery date. **That part already works and isn't going anywhere.**

What's missing is a *different* thing the old system also has: a running, timestamped log where each note is its own separate entry, kept permanently, showing who wrote it and when — more like a comment thread than a single editable text box. The old system has always had both side by side; the new system currently only has the simple box.

**Important reassurance**: building this would not touch or replace the existing notes box in any way — it's a genuinely separate, additional feature, confirmed by checking exactly how the old system uses both. A full implementation plan exists. This is lower priority than Attachments, since the simple notes box already covers the basic need — this would be an enhancement, not filling a total absence.

**Efficiency note, not a decision**: Attachments and this feature are built almost identically under the hood in the old system — same style of storage, same kind of "who added it and when" tracking. If you ever decide to build both, doing them together would likely be more efficient than doing them as two separate projects months apart.

## 3. The Task Board — the daily "what needs doing" screen every employee sees first

This is the most significant of the three, and it's worth understanding why. In the old system, when someone logs in, the very first thing they see — before even the main dashboard — is a screen showing everything that needs attention today, pulled together from across the whole business: overdue follow-ups from CRM, orders stuck waiting on something, production jobs behind schedule, pending approvals, shipping problems. On top of that automatic feed, staff can also create their own manual to-dos, assign them to each other, mark things blocked with a reason, and check things off — a real, actively-used task board, not just a status display.

**This does not exist in the new system at all** — confirmed by a direct, thorough search: nothing anywhere in the new system's code resembles a task or to-do concept. This is a genuine, real gap in daily-use functionality, not a minor or optional feature — it's the literal first screen your team is used to seeing every single day.

**Worth knowing**: earlier in this audit, this specific feature was briefly and incorrectly grouped in with a batch of things assumed to be aspirational ideas that nobody had actually built yet. That was a mistake, caught and corrected directly — this one is real, tested, and in active use in the old system. A full implementation plan now exists, including the note that whoever eventually designs the new system's screens should treat "what do employees see first" as a real, deliberate decision — the old system clearly did.

## What this doesn't affect

- None of these reopen any existing certified/closed status. They're additional scope information, the same as every other finding in this audit.
- Nothing here has been implemented, fixed, or changed yet. This is a findings-and-planning summary, not an action.
- None of these are bugs or design disagreements — all three are simply features from the old system that haven't been built yet in the new one.

## Bottom line

Two of these three (Order Attachments and the Task Board) are routine, everyday functionality your team would notice missing immediately, and both now have complete implementation plans ready for a decision. The third (structured Order Notes) is a real but lower-priority enhancement on top of a notes feature that already works today. If more than one of these is ever authorized, Attachments and Notes are efficient to build together; the Task Board is a separate, larger, and more urgent piece on its own.
