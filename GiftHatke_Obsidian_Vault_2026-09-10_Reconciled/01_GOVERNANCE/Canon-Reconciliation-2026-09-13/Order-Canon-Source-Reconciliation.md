# Domain 1 — Order Canon source reconciliation

Date: 2026-09-13. Domain classification: **partial foundation** for exhaustive Canon parity. Closed Orders implementation slices remain closed.

## Evidence boundary

Active source: `GiftHatkeOS-Standalone@06090ddcd8a68c6be6ef484d43b252547bf5ec55`. Frozen source: `GiftHatkeOS@fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`.

Canon authority: [[Order-Management-Canon-v1.0-Stage-1-Original]], [[Order-Management-Canon-v1.0-Stage-2-Original]], and [[Domain-01-Stage-10-Original]]. This pass compares selected explicit requirements; it does not claim every stage has been semantically reconciled.

## Direct comparison

| Area | Evidence and result | Classification |
|---|---|---|
| Frozen vocabulary preservation | Exact ordered arrays match between frozen Orders.js and standalone order.ts: 18 commercial statuses, 5 payment statuses, 4 priorities, 13 channels, 9 personalization statuses and 5 approval statuses. Mechanical comparison passed for all six arrays. | complete |
| Canon commercial state mapping | Stage 1 §12 separates a commercial lifecycle including Pending Confirmation, In Fulfilment and Closed. Frozen/current ORDER_STATUSES instead includes operational stages such as In Production and Quality Check. Exact literal equivalence is false; a certified semantic mapping or explicit disposition is still needed. This does not prove an incorrect live transition. | partial foundation |
| Canon financial arithmetic | Stage 1 §11 requires fixed-precision arithmetic and forbids floating-point financial totals. Current calculateOrderTotals uses JavaScript Number arithmetic, and roundOrderMoney uses Math.round(value * 100) / 100. PostgreSQL NUMERIC declarations alone do not change the application arithmetic. No observed monetary error is asserted by this source review. | partial foundation |
| Domain ports | order-ports.ts defines CRM, Customer, Product lookup and Production synchronization seams. Port declarations alone are not executable integration proof. | partial foundation |
| CRM and Customer composition | Current server.ts overrides the source-absence CRM and Customer lookups with platformRuntime.crmOrderAdapter and customerOrderAdapter. Customer-aware order mutation resolves/upserts the customer. The earlier source-absence adapter comment saying no CRM/Customer tables exist must not be treated as current runtime truth. | partial foundation |
| Product composition | Current server.ts retains the product member from createOrderLookupSourceAbsenceDependencies; listProductLookups returns an empty collection. createOrderCreateFromLeadService consumes that member. This proves a bounded empty Product lookup at composition, not failure of every lead conversion. | partial foundation |
| Persistence | Current schema and Pack 2.8 evidence include Orders, Items, Activities and sequence state. The richer recovered Stage 2 schema requires semantic mapping; a differing table layout alone is not proof of missing business behavior. | partial foundation |
| Workspace and transport | August 20 mutation-workspace certificate certifies New/Edit/Save/Archive and GET workspace/detail, POST, PUT and DELETE transport usage with expectedRevision and server-owned calculated fields. Historical certificate evidence is preserved; no fresh live exercise or current full route inspection was performed here. | partial foundation |
| Permission history | Pack 2.8 contains an additive correction: mutation-shaped keys already existed in the 109-key catalogue, although that early slice certified only read transport. The later mutation-workspace certificate establishes the later scope. No missing permission or permission #110 is inferred. | complete |
| Tests and live acceptance | Only the six source-array comparisons ran in this pass. Historical test counts remain historical. Cloud browser still has the ERP tab and the earlier Google 502 popup; authentication was not retried and no live Order acceptance is claimed. | partial foundation |

`complete` above applies only to its stated narrow evidence control, not the full Order domain or a new production certification.

## Reconciliation observations

| Observation | Disposition | Classification |
|---|---|---|
| CR-D01-001 — Canon/frozen vocabulary distinction | Map Canon commercial states to the frozen combined workflow without renaming either source. Do not fabricate a one-to-one mapping. | partial foundation |
| CR-D01-002 — Canon arithmetic requirement differs from current implementation | Determine the controlling requirement and any existing accepted variance. Any code remediation requires a separate Titan Lock; this wave only records the difference. | partial foundation |
| CR-D01-003 — Product lookup remains source-absence | Reconcile the controlled Product Master deferral against Domain 21 and current business scope. Do not build Product Master or alter CRM/Orders now. | partial foundation |

These observation IDs supplement the working domain inventory. They do not replace, merge, downgrade or resolve SMP1-GAP-001 through SMP1-GAP-013. The existing handover blockers remain 001, 002, 005, 006, 009 and 011. The final register must retain these observations when determining the eventual exhaustive parity disposition.

## Authority chronology

The August 15 Pack 2.8 foundation certificate describes deferrals at its own baseline. Its later permission correction and the August 20 mutation-workspace certificate must be read together. Current server composition proves that CRM and Customer integration advanced after those early foundation statements. Their earlier absence is not carried forward as a current finding.

Likewise, the mutation-workspace certificate's list of controlled deferrals is historical. This review independently checks only the present Product lookup composition; it does not assert that every listed deferral remains unresolved today.

## Next work and limits

Continue Domain 21 Product ownership/deferral reconciliation and the domain-by-domain semantic comparison. The production recovery/cutover evidence blocker remains open in parallel; it does not prevent further source review.

No application source, module closure, permission, database, deployment or frozen repository was changed. No commit or push occurred. The 44-row matrix remains a working inventory; exhaustive parity and handover are not certified.

Canon Recovery wave: 80% (estimate). User Management: 100% closed/read-only. Settings: 100% closed/current wave. Overall SMP1: less than 100%, handover blocked.
