# Order Attachments — Pre-Handover Implementation Scope

**Date:** 2026-09-15
**Status:** SCOPE DOCUMENT ONLY — NOT AN IMPLEMENTATION AUTHORIZATION
**Origin:** GAP-001, Domain 1 (first/second passes) and the consolidated implementation plan's Tier 1 item 2
**Governs:** pre-handover parity blocker for Order Attachments (Canon §10.4, "Collaboration" — Attachments)

This document is a precise implementation scope, built from a direct, complete read of the frozen source (`OrderWorkspace.js`, all 61 lines), the Canon Domain 1 text, and a systematic check of what already exists in Standalone. It is not a patch and contains no code. Writing or merging anything described here requires its own separate, explicit authorization and its own scope-lock/implementation/tests/regression/closure cycle. Standalone has not been handed over to employees; this is a pre-handover gap, not a live-incident fix.

## 0. Unlike Finance, there is no existing foundation to build on

Before scoping, this pass checked whether Standalone has any Attachment-related domain types, ports, platform code, or routes already scaffolded — the same check that revealed Finance's write-side foundation was already half-built. For Order Attachments, the result is different: a repository-wide search for `attachment` (case-insensitive) across `packages/domain/src`, `packages/platform/src`, and `apps/api/src` returns exactly one match, `FinanceExpense.attachmentUrl` — a single unrelated field on expense records, not an Order concept. A separate search for `OrderNote`, `order_notes`, and `OrderComment` returns nothing at all. **This is a build-from-scratch scope, not a wire-up-the-orchestration-layer scope.**

## 1. Canon grounding

Canon Domain 1 (Order Management), §10.4 "Certified Business Capabilities" → "Collaboration," names four capabilities together: **Comments, Attachments, Notifications, Cross-domain coordination**. Canon does not specify field-level structure for any of them at Stage 10 (the certification stage this report has read; earlier stages that might specify field-level detail were not located — see GAP-001's eleventh pass). This document targets frozen's actual implemented structure as the parity target, consistent with every other scope document in this series.

## 2. What frozen actually implements — read in full, not excerpted

`OrderWorkspace.js` is a single, self-contained 61-line file implementing three related capabilities together: structured Notes (Canon's "Comments"), Attachments, and a combined workspace/timeline view that weaves both together with Activities and Production data. They are tightly coupled in frozen's own design — scoping Attachments in complete isolation from the other two would misrepresent how frozen actually works, so this document covers all three, with Attachments as the primary subject per the operator's request.

### 2.1 Attachments (primary subject)

**Schema**: `Order_Attachments` sheet, headers `Attachment ID, Order ID, Created At, Category, File Name, URL, Source, Description, Created By, Record Status`.

**Write** (`orderWorkspaceAddAttachment`): acquires `LockService.getDocumentLock()` (10-second timeout — shorter than Orders' own 30-second lock elsewhere in the codebase). Requires `orderId` and confirms the order exists. Requires `url` and validates it starts with `https://` (case-insensitive) — the only real validation rule in this function. Defaults: `fileName` → `"Attachment"`, `category` → `"Other"`, `source` → `"Manual"`, `description` → empty string. Actor derived from the active session. Generates an ID (`"OA-" + 8-char UUID slice, uppercased`). Appends the row with `Record Status = "Active"`. Logs an order activity (`"Attachment Added"`). Invalidates the workspace cache. Returns `{id: attachmentId}`.

**Read** (`orderWorkspaceReadAttachments_`): filters by `orderId` and excludes `Record Status === "Archived"`, maps to a plain object, returns newest-first (`.reverse()` on row-insertion order).

**No delete or archive function was found for attachments** in this file — frozen appears to have no way to remove an attachment once added, only to add new ones. This was not exhaustively re-verified against the rest of the frozen repository in this pass (a generic `Record Status` archival mechanism exists elsewhere in the codebase, and it's plausible some other function or manual sheet edit could set it, but no dedicated `orderWorkspaceArchiveAttachment`-style function exists in this file or was found searching for one).

### 2.2 Notes (Canon's "Comments" — included for completeness, not the primary subject)

**Schema**: `Order_Notes` sheet, headers `Note ID, Order ID, Created At, Note Type, Note, Created By, Record Status`.

**Write** (`orderWorkspaceAddNote`): same lock/pattern as attachments. Requires `orderId` (order must exist) and `note` text. `type` defaults to `"Internal"`. Generates an ID (`"ON-" + 8-char UUID slice`). Logs an order activity (`"Note Added"`). Same cache invalidation.

**Read** (`orderWorkspaceReadNotes_`): same filter/exclude/reverse pattern as attachments.

**Relationship to Standalone's existing scalar `notes` field**: Standalone's `Order` domain type already has a single `notes: string` field (confirmed in this report's very first pass), which every order update overwrites wholesale. This is a fundamentally different data shape from frozen's append-only, typed, multi-row Notes — not a smaller version of the same thing. Migrating from a scalar field to a structured collection is a real design decision (does the existing scalar field get deprecated? Migrated into the new table as a single legacy row? Left alone as a separate, simpler field?) that this document does not make. **If Notes/Comments implementation is ever authorized, it should be scoped as its own document** — it is flagged here only because of its tight coupling to Attachments in frozen's own file structure, not because this document is proposing to build it.

### 2.3 The combined workspace and timeline (context, not in this scope)

`orderGetCompleteWorkspace` returns `order, items, activities, notes, attachments, production, financial, timeline, generatedAt` in one call — a single "everything about this order" view. `orderWorkspaceBuildTimeline_` interleaves order-creation, activities, notes, attachments, and production start/completion events into one descending-sorted feed. This is a separate, larger capability than attachments alone (already flagged as its own deferred item across GAP-001's early passes) and is **not scoped for implementation by this document** — noted only so whoever picks this up understands that building attachments alone does not, by itself, give employees frozen's combined order-detail view; that would need its own follow-on scope once Notes and a timeline-assembly capability also exist.

## 3. Proposed Standalone scope for Attachments

Given §0 (no existing foundation), this needs the full stack, not just an orchestration layer:

- **Domain type**: `OrderAttachment` in `packages/domain/src/order.ts` (or a new `order-attachments.ts` if the existing file is judged too large already — precedent for splitting exists elsewhere, e.g. Procurement's PR/PO/GRN are separate files despite being one domain). Fields matching §2.1's schema: `attachmentId, orderId, createdAt, category, fileName, url, source, description, createdBy, recordStatus`.
- **Repository port**: a new `OrderAttachmentRepositoryPort` (or extend `order-ports.ts`'s existing `OrderCustomerPort`-style pattern) with `listAttachments(orderId)` and `addAttachment(input)` — matching the shape of frozen's own two functions, not inventing additional capability frozen doesn't have (no update/delete, per §2.1's finding that frozen has none either).
- **Platform implementation**: new file or addition to `packages/platform/src/order.ts`, following the same `DatabaseRuntime`-parameterized factory-function pattern used throughout this file and `customer.ts`.
- **Migration**: a new `order_attachments` table (or equivalent) in `packages/database/src`, following the existing migration-file convention (`packages/database/migrations`, one timestamped file per foundation, per the pattern already used for every other domain's tables).
- **Application-service method**: `addAttachment(input)` on the Order application service (`apps/api/src/order-service.ts`), reusing the existing `findOrderById`/`getOrder` check for order existence, the same `https://` URL validation frozen enforces, and the same actor-derivation pattern already used throughout this codebase's other mutations.
- **Route**: `POST /orders/:orderId/attachments` in `apps/api/src/routes/orders.ts`, CSRF-protected and permission-gated the same way every other Order mutation route already is (confirm the exact permission key against the existing catalogue — Orders' existing keys are `orders.orders.create`/`orders.orders.update`/`orders.orders.delete`; a new `orders.attachments.create`-shaped key, or reuse of `orders.orders.update`, is a decision for whoever scopes the actual route, not decided here).
- **Read exposure**: a `GET /orders/:orderId/attachments` route, or — the smaller, more contained option — including attachments in the response of the existing `GET /orders/:orderId` route by extending `OrderAggregateSnapshot` (currently `{order, items}` only, confirmed by direct read of `packages/platform/src/order.ts`). **Recommend the separate-endpoint approach** to avoid touching `OrderAggregateSnapshot`, which is used by every order read path throughout the codebase (create, update, archive, and get all return or consume this same shape) — extending it is a wider-blast-radius change than adding one new, narrowly-scoped endpoint. This is a recommendation, not a decision made here.

## 4. Cross-cutting: no atomicity concern here, unlike the Finance/Orders/CRM/Procurement findings

Unlike every "convert-then-mark-source" or "write-A-then-write-B" finding elsewhere in this audit, adding an attachment is a single, self-contained write (one new row, one activity log entry, one cache invalidation) with no second system to fall out of sync with. The transaction-atomicity lessons from this report's other scope documents (Finance, and the proven Procurement pattern) do not directly apply here — there is no equivalent ordering hazard to design around. The one thing worth deciding deliberately, rather than defaulting to frozen's shape: whether the activity-log entry and the attachment row should be written in the same transaction (cheap insurance, low cost) even though frozen itself doesn't provide that guarantee (Apps Script can't), the same "use the technology Standalone actually has" principle already applied in the Finance and Procurement scope documents.

## 5. Explicitly out of scope for this document

- Structured Notes/Comments (§2.2) — flagged, not scoped, pending its own authorization and its own decision about the existing scalar `notes` field's future.
- The combined workspace/timeline view (§2.3) — a separate, larger, already-previously-deferred capability.
- Any delete/archive capability for attachments — frozen doesn't have one; adding one would be new capability beyond frozen parity, a separate decision.
- The exact permission key naming — flagged, not assigned.
- File upload/storage (frozen's attachments are URL references to externally-hosted files, e.g. Cloudinary per this codebase's existing Personalization asset pattern — not a file-upload endpoint of Standalone's own; this document assumes the same URL-reference model, not a new file-hosting capability).

## 6. Suggested sequencing, not an authorization

If authorized: (1) domain type + migration + platform implementation + repository port, (2) application-service method + write route, (3) read route (or the wider `OrderAggregateSnapshot` extension, if that broader path is chosen instead). Notes/Comments and the combined timeline, if ever authorized, would naturally follow the same three-step shape once Attachments has proven the pattern for this specific domain — but that is a separate future scope, not committed to here.

## 7. Implementation record, 2026-09-15

Authorized and implemented in this session, following §6's sequencing. **Nothing has been committed or pushed** — this exists in the working tree pending separate explicit authorization, same as every other implemented item this session.

**Built:** `OrderAttachment` domain type and `OrderAttachmentPort` (`packages/domain`); a Postgres migration for `order_attachments` (FK to `orders` with cascade delete, a check constraint enforcing the `https://` rule at the database level too, not just in application code); a platform adapter that writes the attachment row and an "Attachment Added" activity-log entry inside one shared transaction (§4's recommendation, taken up); a thin application-service validation layer; and a dedicated route plugin (`GET`/`POST /orders/:orderId/attachments`), registered independently of the main Orders route file to keep blast radius small, matching §3's read-exposure recommendation.

**One deviation from §3, necessary rather than optional:** frozen's own attachment-ID prefix is `"OA-"`, but Standalone's existing `order_activities` table already uses `"OA-"` for a different, pre-existing concept (Order Activity log entries) — confirmed by direct read of the order-foundation migration before choosing a prefix. Reusing `"OA-"` for attachments would have collided with that existing ID space, so attachments use `"ATT-"` instead. The ID-generation *method* is otherwise identical to frozen's (an 8-character uppercase hex slice of a random UUID) — only the prefix differs, and only because of this Standalone-internal naming conflict, not a design choice made freely.

**Permission key:** §3 flagged this as undecided between a new `attachments`-shaped key or reusing `orders.orders.update`. Resolved as a new key, `orders.attachments.create`/`.read`, added to the Orders module of the permission catalogue — consistent with how `payments` and `personalization` are already separate feature groups within Orders rather than folded into the generic `orders` key.

**Verification:** 23 new tests across three files (platform adapter — source-shape verification, since no existing precedent in this codebase unit-tests `order.ts`'s other transactional functions against a full Kysely mock either; application service — full runtime behavior with a mock port; routes — source-shape verification matching the existing `customer-approval-routes.test.mjs` convention). Full monorepo typecheck clean. Full regression suite re-run: platform 207/207, domain 226/226, api 557/558 — the one failure is the same pre-existing, unrelated `reports-service.ts` timezone issue flagged during PHB-1, not something this work touched.

**Explicitly still out of scope, per §5, unchanged:** structured Notes/Comments (PHB-4, separately scoped), the combined workspace/timeline view, delete/archive capability, and file upload/storage (this remains a URL-reference model, matching frozen).
