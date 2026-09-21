# Dashboard & Reporting — What the Audit Found

**Date:** 2026-09-15
**Covers:** the Executive Dashboard (the landing screen showing today's numbers) and the deeper per-module analytics/reporting behind it, as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 7 & 30 sections), `Executive-Dashboard-Pre-Handover-Scope-2026-09-15.md`, and `Executive-Business-Intelligence-Pre-Handover-Scope-2026-09-15.md`, if anyone wants to check the underlying evidence.

## The short version

This is the module where the old and new systems differ the most, and it splits into two genuinely different kinds of gap. The main landing screen — the numbers and alerts an employee sees right after logging in — is real, everyday content in the old system with essentially nothing built yet in the new one; that one matters for day-one usability. The deeper analytics behind it (sales trends, production bottlenecks, customer lifetime value, and so on) are also missing from the new system, but it turns out the old system's own employees can't actually reach most of that either — it was built but never wired into the old system's actual menus. Both pieces now have a precise, ready-to-build implementation plan.

## 1. The landing screen itself is mostly missing from the new system

In the old system, the screen an employee sees on login (right after a simpler "today's tasks" screen, covered separately) shows: 7 headline numbers (today's sales, this month's revenue, orders today, average order value, gross profit, net profit, cash position), a greeting, a list of things needing attention across every module, a queue of pending approvals, a feed of recent activity, and a health indicator showing whether every part of the system is reporting in correctly.

The new system's equivalent screen currently shows none of this — just a raw combination of order, production, and inventory data with no summary numbers, no alerts feed, no approvals queue.

**The good news, found while scoping this**: the old system's own landing screen turns out to be built on a documented, precise checklist of exactly which numbers are "must-have" (the ones that produce the 7 headline cards) versus "nice-to-have" (the alerts/activity/trend content). That checklist gives the new system's build a clear, staged target — get the must-have numbers working first, since those are what employees actually look at first thing every day; the nice-to-have layer can follow. A full implementation plan following exactly this staging now exists.

## 2. The deeper analytics behind it are missing too — but with an important asterisk

Beyond the landing screen, the old system separately has real, working analytics for six different areas: sales trends and top products/customers, production bottlenecks and delivery risk, inventory stock health and reorder recommendations, shipping performance and delivery exceptions, customer relationship health (who's overdue for follow-up, who's your most valuable repeat customer), and a cross-module risk summary pulling the most urgent issues from all of the above into one list.

The new system doesn't compute any of this yet either — with one exception: the financial version of this (profitability, expense patterns, overdue payments, where profit is quietly leaking) is already fully built and working in the new system today. That one's done.

**The asterisk**: checked directly, none of the other five analytics screens in the old system are actually reachable by your own employees today, through the app's normal menus. They were built, tested, and each has its own dedicated screen — but none of those screens were ever connected to the old system's actual navigation. So while this is real, valuable capability worth eventually having, it's not something your team is using today and would visibly miss on day one the way the landing screen (item 1) would be missed. That's a meaningful difference in how urgent this piece is, even though the underlying gap is real either way.

**One easy, separate fix worth knowing about**: one small piece of this — the cross-module risk summary — is currently broken in the new system by a single line of code that was never connected to anything, independent of whether the fuller analytics work ever happens. That's a near-zero-effort fix on its own, whenever you'd want it done.

A full implementation plan for all six analytics areas now exists, including which one is cheapest to start with (the customer-relationship one, since its data-reading connection is already half-built) and how it should be sequenced against the landing-screen work in item 1, since the two overlap in places and shouldn't be built twice.

## What this doesn't affect

- Nothing here reopens any existing certified/closed status for other modules. This is new scope information about Dashboard/Reporting specifically.
- Nothing here has been implemented, fixed, or changed yet. This is a findings-and-planning summary, not an action.
- This isn't a sign the new system's engineering is behind — this module is simply the largest genuinely new build of everything found in the audit, not a case of something being done wrong.

## Bottom line

Two things worth building, both fully scoped and ready for a decision: the everyday landing screen (worth prioritizing, since it's what employees see first) and the deeper six-area analytics layer (real and valuable, but lower urgency, since even the old system's own team can't currently reach most of it). One of the six analytics pieces has a near-free one-line fix available independent of everything else. The financial analytics piece is the one part of this already fully done.
