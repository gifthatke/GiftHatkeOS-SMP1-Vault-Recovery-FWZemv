# Domain 11 — Evidence Card

WORKFLOW, PROCESS AUTOMATION & ORCHESTRATION ENTERPRISE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-11-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `2d5fc0b7-8913-4ef7-8528-b9d030aa88fd`; SHA-256 `53ae15ee2c130c9b7649cb72165d879e71fef908d8b569e26d20f149d9e5f587`. Bounded approval, handoff and synchronization behavior does not prove a general orchestration platform (GAP-012).

## Frozen Apps Script behavior

[`ApprovalService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ApprovalService.js) (`759052731cf70175d34e9809ca447a6923a5f347`); [`GLP1ProductionArtworkHandoffHotfixTests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/GLP1ProductionArtworkHandoffHotfixTests.js) (`6bfd35f07296b1ab18cf7f3e9e4a18d370f2befd`); [`WorkflowService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/WorkflowService.js) (`31a73ffee644a988850e96b55b1c416c130bfcdc`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/customer-approval.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/customer-approval.ts) (`0095df703f960e85571db28e75bfc1c46a0cdfe9`)

## Database and persistence

[`packages/database/src/customer-approval-persistence.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/customer-approval-persistence.ts) (`7432a3508c3cc766d987dee6b556bcc101ba4a3b`)

Declared matching table candidates: procurement_approval_thresholds, finance_approvals, customer_approval_requests, customer_approval_activities. Schema declarations do not attest deployed data or migration success.

## Ports

No matching path in this bounded inventory; semantic absence is not established.

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/customer-approval.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/customer-approval.ts) (`324abd276f04406886e2fcac512289fd2b4cf65f`)

## Services and routes

[`apps/api/src/customer-approval-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/customer-approval-service.ts) (`ef9b949b3fe20406761265fec8d9bcb2f420d13d`); [`apps/api/src/production-order-synchronization-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/production-order-synchronization-service.ts) (`80201ced78b816e9c45aefcf1aca5b6b84e0c63b`); [`apps/api/src/production-synchronization-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/production-synchronization-service.ts) (`f60908383717b1767ebd90f7b68b6aae10b83684`); [`apps/api/src/routes/customer-approval.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/customer-approval.ts) (`a8f31d9d83d0c14876407c02b7416e1b2328ef1b`); [`apps/api/src/shipping-order-handoff.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/shipping-order-handoff.ts) (`40242c99316a686ad7a71c43a012c9f0e7a97484`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/customer-approval-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/customer-approval-api.ts) (`c093ae786de26835ad8a1db5f651b07c190c7094`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/customer-approval-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/customer-approval-routes.test.mjs) (`7090636e374220db364f645823924943308ed4fc`); [`apps/api/test/customer-approval-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/customer-approval-service.test.mjs) (`a37601d8465a5f0883c642e91281b8b140d19aec`); [`apps/api/test/production-order-synchronization-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/production-order-synchronization-service.test.mjs) (`4eae4266a33c3c659f39f7e7fcbc6cbd9b288c52`); [`apps/api/test/production-synchronization-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/production-synchronization-service.test.mjs) (`c599f097767d26762ea9caddbfc682bbd80ee896`); [`apps/api/test/shipping-order-handoff.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/shipping-order-handoff.test.mjs) (`2e8f8bde325766d18ca9d3af577252c9f4bd047f`); [`apps/web/test/personalization-cloudinary-approval.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/personalization-cloudinary-approval.test.mjs) (`259dbcec9c7792bd2f31318a36c0e154b7a9cd1d`); [`packages/database/test/customer-approval-persistence.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/customer-approval-persistence.test.mjs) (`23cd5cd8ba00023beb9630f95b16b157023ee8e0`); [`packages/domain/test/customer-approval.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/customer-approval.test.mjs) (`b032f3606de3ff63be4891916b482e70eefb627f`); [`packages/platform/test/customer-approval.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/customer-approval.test.mjs) (`0c89c26e3d0dd672b8db6c5e5a3254e97cde459f`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-production-order-synchronization-implementation-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-implementation-certification.md) (`5424ba72629e4c5907ede05ee547488383f4cde5`); [`docs/governance/smp1-production-order-synchronization-ownership-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-ownership-certification.md) (`158c8cf052f21b9b5ee43788c6f4ce68a8c9f359`); [`docs/governance/smp1-production-order-synchronization-ownership.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-ownership.md) (`03ddf10101d8f3e73bb39a12c7f7dce1118c1f3f`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage formally certifies the **Workflow, Process Automation & Orchestration Domain** as the permanent enterprise process coordination foundation within the GiftHatkeOS Enterprise Canon.

It consolidates the previous nine stages into one authoritative specification governing:

- Enterprise business processes.
- Workflow definitions.
- Workflow execution.
- Human tasks.
- System tasks.
- Business rules.
- Automation governance.
- SLA governance.
- Exception governance.
- Cross-domain orchestration.
- Process intelligence.
- Enterprise workflow stewardship.

This certification confirms that the domain has achieved:

- Enterprise completeness.
- Process consistency.
- Governance maturity.
- Technology independence.
- Long-term architectural stability.
- Cross-domain integration readiness.

This document becomes the permanent reference for every future implementation of enterprise workflow management within GiftHatkeOS.

---
