# Order Notes (Structured Comments) — Pre-Handover Implementation Scope

**Date:** 2026-09-15
**Status:** SCOPE DOCUMENT ONLY — NOT AN IMPLEMENTATION AUTHORIZATION
**Origin:** GAP-001, Domain 1 (first/second passes), flagged as a related-but-unscoped item in the Order Attachments scope document (§2.2)
**Governs:** pre-handover parity blocker for structured Order Notes (Canon §10.4, "Collaboration" — Comments)

This document is a precise implementation scope, built from the same complete read of `OrderWorkspace.js` used for the Attachments scope, plus a direct trace of Standalone's existing scalar `notes` field through the codebase to resolve a question the Attachments document left open. It is not a patch and contains no code. Writing or merging anything described here requires its own separate, explicit authorization and its own scope-lock/implementation/tests/regression/closure cycle. Standalone has not been handed over to employees; this is a pre-handover gap, not a live-incident fix.

## 0. The question this document resolves: is this a migration, or an addition?

The Attachments scope document flagged an open question: Standalone's `Order.notes` scalar field — is structured Notes a replacement for it, a migration target, or something separate? This pass traced both sides and the answer is clean: **frozen has both, as two genuinely separate things, and Standalone should too.**

Frozen's `Orders.js` `ORDER_HEADERS` includes a plain `"Notes"` column on the Order row itself (index 20, alongside `Photo Link`, `Personalisation`, `Assigned To`) — a single scalar field, set via `cleanText(payload.notes)` on every ordinary order save, the same way any other order field is edited. This is a **separate concept** from `OrderWorkspace.js`'s `Order_Notes` sheet — a distinct, typed, append-only, multi-row collection with its own IDs, types, and history.

Standalone's existing `notes: string` field (confirmed present on the `Order` domain type since this report's first pass) is the faithful, correct parity match for frozen's scalar `Order.Notes` column — not a weaker version of structured Notes. Direct trace confirms it is genuinely live and employee-facing today: `apps/web/src/orders.ts` renders it as a `<textarea name="notes" rows="3">` on the order edit form (line 673) and displays it in an order detail list (`["notes", order.notes]`, line 2048); `order-api.ts` and `order-mutation-api.ts` both carry it through the web transport layer as an ordinary field.

**Conclusion: implementing structured Notes should not touch, deprecate, or migrate the existing scalar `notes` field.** It is correct as-is and already in active use for its own purpose (a quick, single-value remark editable inline with the rest of the order). Structured Notes is new, additive capability — a second, richer thing living alongside it, exactly as frozen has both.

## 1. Canon grounding

Same as the Attachments document: Canon Domain 1, §10.4 "Collaboration," names **Comments** alongside Attachments, Notifications, and Cross-domain coordination, without specifying field-level structure at Stage 10. This document targets frozen's actual implemented structure as the parity target.

## 2. What frozen actually implements

**Schema**: `Order_Notes` sheet, headers `Note ID, Order ID, Created At, Note Type, Note, Created By, Record Status`.

**Write** (`orderWorkspaceAddNote`, `OrderWorkspace.js`): acquires `LockService.getDocumentLock()` (10-second timeout, same as Attachments). Requires `orderId` and confirms the order exists. Requires non-blank `note` text — the only content validation. `type` defaults to `"Internal"` (frozen does not enumerate or validate against a fixed type list in this function — any string is accepted, the same unchecked-string pattern already flagged for Finance's Budget `Status` field in the Finance scope document). Actor derived from the active session. Generates an ID (`"ON-" + 8-char UUID slice, uppercased`). Appends the row with `Record Status = "Active"`. Logs an order activity (`"Note Added"`). Invalidates the workspace cache. Returns `{id: noteId}`.

**Read** (`orderWorkspaceReadNotes_`): filters by `orderId`, excludes `Record Status === "Archived"`, maps to a plain object (`id, orderId, createdAt, type, note, createdBy` — defaulting a missing type back to `"Internal"` on read as well as on write), returns newest-first.

**No delete, archive, or edit function was found for notes** in this file, the same as Attachments — frozen supports adding notes, not removing or correcting them, at least not through this file. Consistent with an append-only comment-history model, which is arguably the intended design (a corrected note should probably be a new note, not a silently-edited old one) rather than a gap — this document does not recommend adding edit/delete capability beyond frozen's behavior without a deliberate decision to do so.

## 3. What already exists in Standalone

A repository-wide search for `OrderNote`, `order_notes`, and `OrderComment` (case-insensitive) across `packages/domain/src`, `packages/platform/src`, and `apps/api/src` returns nothing (confirmed in the Attachments scope document's investigation, not re-run here). As with Attachments, **this is a build-from-scratch scope**, aside from the already-correct, already-live scalar `notes` field described in §0, which this scope leaves untouched.

## 4. Proposed Standalone scope

This should closely mirror the Attachments scope document's shape, given the two capabilities are structurally identical in frozen (same file, same lock pattern, same ID-generation style, same read/write shape) — differing only in their fields and validation rule:

- **Domain type**: `OrderNote` (fields: `noteId, orderId, createdAt, type, note, createdBy, recordStatus`), placed alongside wherever `OrderAttachment` ends up (same file or sibling file — see the Attachments document's note on file-splitting precedent).
- **Repository port**: `listNotes(orderId)` and `addNote(input)`, matching frozen's two functions exactly — no update/delete, per §2's finding.
- **Platform implementation**: same `DatabaseRuntime`-parameterized factory pattern as the rest of `packages/platform/src/order.ts`.
- **Migration**: a new `order_notes` table, following the existing per-domain migration-file convention. If Attachments (PHB-3) is implemented first, this is a natural, small follow-on migration in the same style, not a new pattern to design.
- **Application-service method**: `addNote(input)` on the Order application service, requiring non-blank note text (frozen's only real validation), reusing the existing order-existence check and actor-derivation pattern.
- **Route**: `POST /orders/:orderId/notes`, CSRF-protected and permission-gated the same way as the rest of Orders' mutation routes (exact permission key not assigned here, per the same reasoning as the Attachments document — confirm against the existing catalogue rather than inventing one; note that `orders.attachments.create` and `orders.notes.create`, if that granularity is chosen, would be two new keys rather than one, which is itself a decision — a single shared `orders.collaboration.create`-style key covering both Attachments and Notes is also a reasonable alternative, since frozen itself does not appear to distinguish permission-wise between the two at the file level examined).
- **Read exposure**: same recommendation as Attachments — a separate `GET /orders/:orderId/notes` endpoint rather than extending the shared `OrderAggregateSnapshot`, for the same blast-radius reasoning already given in that document.
- **Type validation**: frozen accepts any string for `type`, defaulting to `"Internal"`. A new Standalone implementation could reasonably enumerate a fixed type list (the same pattern Procurement's `PROCUREMENT_STATUS_TRANSITIONS` demonstrates is achievable in this codebase) rather than reproducing frozen's unchecked-string weakness — but, consistent with every other scope document in this series, that is a design choice for whoever scopes the actual implementation, not decided here. Frozen's own UI/usage would need to be checked for what type values are actually used in practice (this pass did not locate a client-side type picker or enumerated list anywhere in the frozen file set) before choosing a fixed list, to avoid inventing categories that don't match real usage.

## 5. Cross-cutting: same atomicity note as Attachments

Adding a note is a single, self-contained write (one row, one activity log entry, one cache invalidation) — no second system to fall out of sync with, and none of the "convert-then-mark-source" atomicity hazards found elsewhere in this audit apply here. The same "write the activity log entry in the same transaction as the note row, since Standalone's Postgres foundation makes that free, even though frozen's Sheets foundation couldn't guarantee it" principle from the Attachments document applies equally here.

## 6. Explicitly out of scope for this document

- Any change to the existing scalar `notes` field — resolved in §0 as correct and out of scope, not merely deferred.
- The combined workspace/timeline view — same as the Attachments document, a separate, larger, already-previously-deferred capability that would naturally consume both Attachments and Notes once both exist, but is not itself scoped by either document.
- Edit or delete capability for notes — frozen doesn't have it; adding it would be new capability beyond parity.
- The exact permission-key design (shared vs. separate keys for Attachments and Notes) — flagged with a recommendation-shaped observation, not decided.
- An enumerated `type` list — flagged as a reasonable improvement opportunity, not decided, pending a check of what type values frozen's UI actually offers today (not located in this pass).

## 7. Suggested sequencing, not an authorization

If both Attachments (PHB-3) and Notes are ever authorized, implementing them together (or immediately back-to-back) is likely more efficient than fully separating them — they share the same lock pattern, the same ID-generation style, the same order-existence check, and plausibly the same permission-key design decision — rather than solving the same small set of design questions twice. This is a sequencing observation, not a commitment; each still needs its own explicit authorization.

## 8. Implementation record, 2026-09-15

Authorized and implemented in this session, following §7's observation — PHB-3 (Order Attachments) was already implemented earlier in this session, so this reused its established shape directly rather than rediscovering it. **Nothing has been committed or pushed.**

**Built:** `OrderNote` domain type and `OrderNotePort` (`packages/domain/src/order-note.ts`, `order-ports.ts`); a Postgres migration for `order_notes` (FK to `orders` with cascade delete, `select`/`insert` grants only — no update/delete, matching frozen); a platform adapter that writes the note row and a "Note Added" activity-log entry inside one shared transaction, mirroring Attachments' atomicity upgrade over frozen's two-separate-writes shape; a thin application-service validation layer requiring only non-blank note text (frozen's one real validation rule); routes `GET`/`POST /orders/:orderId/notes`.

**§4's two open design questions, resolved:**
- **Permission keys**: separate `orders.notes.read`/`.create` keys, following the precedent PHB-3 already established for `orders.attachments.*` rather than inventing a shared `orders.collaboration.*` key — consistent with this codebase's existing convention of one feature key per Orders sub-capability (`payments`, `personalization`, `attachments` are already separate).
- **Type validation**: kept as frozen has it — a free-text `note_type` column defaulting to `"Internal"`, no enumerated list. §4 itself noted no client-side type picker was found anywhere in frozen to base a fixed list on; inventing categories without knowing real usage risked getting it wrong, so this ports frozen's actual (if unchecked) behavior faithfully rather than "fixing" it unasked.

**No ID-prefix collision, unlike Attachments:** frozen's own `"ON-"` prefix was checked directly against every existing ID space in Standalone before use (unlike `"OA-"`, which collided with the pre-existing Order Activity log) and found free — notes use `"ON-"` exactly as frozen does, no substitution needed.

**Verification:** 23 new tests across three files (platform adapter — source-shape verification, matching Attachments' established convention; application service — full runtime behavior with a mock port; routes — source-shape verification). Full monorepo build clean. Full regression suite re-run after updating the permission-catalogue certification test's key-count/hash for the two new `orders.notes.*` keys (same routine drift-check this session has done after every catalogue change): domain 260/260, platform 231/231, api 632/633 — the one failure is the same pre-existing, unrelated `reports-service.ts` timezone issue tracked since PHB-1.

**Explicitly still out of scope, per §6, unchanged:** the existing scalar `notes` field (untouched, confirmed correct as-is in §0), the combined workspace/timeline view, edit/delete capability for notes.
