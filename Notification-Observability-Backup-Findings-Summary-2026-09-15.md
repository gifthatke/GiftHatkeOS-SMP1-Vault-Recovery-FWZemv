# Notification, Observability & Backup — What the Audit Found

**Date:** 2026-09-15
**Covers:** whether the system sends notifications, whether anyone would be alerted if something technical went wrong, and how backups/recovery are handled — as reviewed under GAP-001 (the ongoing source-code audit comparing your old system, your new system, and the original business blueprint)
**Status:** informational summary — nothing here is a certification, a bug ticket, or a decision. It's meant to give you what's needed to decide whether anything requires action.

Full technical detail lives in `GAP-001-Source-Integrity-and-Orders-Reconciliation-2026-09-14.md` (Domain 9 and 14 sections, and the Backup/Recovery pass within the expanded-scope sweep), if anyone wants to check the underlying evidence.

**Note**: one item here (the health-check finding) was already mentioned briefly in the Identity, Security & Settings summary. It gets the fuller explanation here, since this is where it actually belongs.

## The short version

None of this is about the business features your team uses day to day — everything here is more "behind the scenes." The main honest finding is that **neither** the old system nor the new one actually sends real notifications (no emails, no texts) despite having some notification-related code — that's not something the migration changed, it was never really there. The rest is a small technical logging gap and one operational note worth someone following up on, unrelated to how your team would use the software.

## 1. Neither system actually sends emails, texts, or push notifications — and that's not new

Both systems have code that *looks* like a notification feature — records get created with titles, messages, severity levels — but a thorough check of both codebases found **zero** actual outbound sending capability anywhere. No emails go out, no texts go out, nothing gets pushed to a phone, in either the old system or the new one. What both actually do is just save a record that something happened, for someone to look at later inside the app itself.

This is not a migration issue — it's identical behavior in both systems, so nothing was lost. It's included here because "notifications" sounds like it should mean "the system tells someone something," and it's worth being clear that neither system has ever actually done that.

**One small, one-sided piece**: the old system has a small internal feature that watches technical performance numbers and creates alert records if something crosses a threshold — the new system doesn't have an equivalent. But checked directly, this isn't something your team ever saw either — it's not connected to any screen in the old system, only used internally by a performance-monitoring routine. Not flagged as something worth fixing; recorded for completeness only.

## 2. The "is everything working" check doesn't actually check anything yet

The new system has a basic endpoint meant to answer "is the system up and running okay" — the kind of thing a hosting service pings periodically. Right now, it always answers "yes, everything's fine" without actually checking anything, even though a real check (confirming the database is actually reachable) exists elsewhere in the code and just isn't connected to this specific check yet.

There's a more capable, properly-built health-checking system underneath this, but it was deliberately left unconnected to the outside world for now — there's a note directly in the code saying so, suggesting this was an intentional choice for this stage of the project rather than something forgotten.

**What this means for you**: this doesn't affect how your team uses the software at all — it's purely about whether the people responsible for keeping the system running smoothly have a reliable way to know if something's actually broken. Worth a quick confirmation from whoever manages hosting that this is expected for now, and getting connected before the system is fully relied upon.

## 3. Backups: both systems currently only keep records *about* backups, not real backups

This is worth being precise about, since "backup" can sound alarming if misread. Both the old and new systems have code for tracking backup metadata — a record saying "a backup happened, here's when, here's a reference to where" — but neither system's code actually *performs* the backup itself or *performs* a restore. In the old system, this tracking code isn't even connected to anything that runs automatically. This is identical in both systems, so again, nothing was lost in the migration — this was never a fully automated, self-contained backup feature in the old system either.

**One thing the new system does add**: a genuine "practice restore" check — it can validate that a backup file looks structurally correct and confirm nothing would have changed if it were restored, without actually touching any real data. The old system has nothing like this. A small, real improvement, though it doesn't change the core point above.

This connects to two already-known, pre-existing open items about how backups and disaster-recovery are actually handled for the live hosted system (separate from this audit, already tracked elsewhere) — not something newly discovered here, just confirmed to still be relevant.

## What this doesn't affect

- Nothing here affects any business feature your team uses — Orders, CRM, Production, Finance, and everything else covered in earlier summaries are unrelated to this document.
- Nothing here reopens any existing certified/closed status.
- Nothing here has been implemented, fixed, or changed. This is a findings summary, not an action.

## Bottom line

The one genuinely useful thing to take from this: neither system sends real notifications despite looking like it might, so don't assume anyone gets pinged automatically about anything. Everything else here is a technical/operational note for whoever manages the hosted system, not something that needs a decision from you about how the business runs.
