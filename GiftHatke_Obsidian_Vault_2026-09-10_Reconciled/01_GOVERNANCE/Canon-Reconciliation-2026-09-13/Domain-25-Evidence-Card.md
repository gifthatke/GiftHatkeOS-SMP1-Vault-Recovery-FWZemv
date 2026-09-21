# Domain 25 — Evidence Card

ENTERPRISE HUMAN CAPITAL, WORKFORCE & ORGANIZATIONAL MANAGEMENT CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-25-Stage-10-Original]]; original certification state: **Certification Status:** ✅ CERTIFIED. Message `b0c32b7c-c95c-42fe-ba1c-d112681a6310`; SHA-256 `8ae3b972f4c76e291b06b22b396418dfe81f9f7dd51fd739507c4e6d0c42b9c5`. Users, roles and organization settings are foundations; do not infer HR lifecycle workflows from them.

## Frozen Apps Script behavior

[`ERP73RoleAnalyticsApi.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleAnalyticsApi.js) (`ec905c185b37d99c76a0db3718a156f839c1b6da`); [`ERP73RoleAnalyticsService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleAnalyticsService.js) (`4e85274756fca9938b0a8921acc0a14c386a2fd4`); [`ERP73RoleApiContracts.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiContracts.js) (`9dc55b8a084b7d5bc4c04a7719fefeae096f63e6`); [`ERP73RoleApiSprint31Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint31Integration.js) (`dc7e360de23287a8e0b8f53ed6e06d4b5a5d5f5d`); [`ERP73RoleApiSprint31Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint31Tests.js) (`27a6c2e369dd56a7376265843be258f50c207da1`); [`ERP73RoleApiSprint32Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint32Integration.js) (`d42a3f113e06d48dcacc2dd938d342883e8086e8`); [`ERP73RoleApiSprint32Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint32Tests.js) (`9468db03a25f0c7a79b983a83cec2a90040e4341`); [`ERP73RoleApiSprint33Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint33Integration.js) (`fd34dff190ef7b81a21246ef60630a159cc712eb`); [`ERP73RoleApiSprint33Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint33Tests.js) (`a9c2784a6e00dc00a4460847827074f4b7a3115b`); [`ERP73RoleApiSprint34Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint34Integration.js) (`29de3afc354115a88321ed7c718c6685292a56c9`); [`ERP73RoleApiSprint34Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint34Tests.js) (`b6e1b5ad9c169d0ce44f4e9609f6933a264d8d38`); [`ERP73RoleApiSprint35Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint35Integration.js) (`6f19d8e2f400046d852eb5921d19017e0c914756`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/user-management.ts) (`18368656833ef5e338d561fb5112ef7afda04653`)

## Database and persistence

No matching path in this bounded inventory; semantic absence is not established.

Declared matching table candidates: organization_branches, organization_departments, organization_cost_centres, roles, role_permissions, user_role_assignments. Schema declarations do not attest deployed data or migration success.

## Ports

No matching path in this bounded inventory; semantic absence is not established.

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/user-management-lifecycle.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/user-management-lifecycle.ts) (`bd45f5403d249aad521d68b3efc9884e71b07a30`); [`packages/platform/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/user-management.ts) (`bda6751dbf24673333c2eb37aa1a7c50c16f42b7`)

## Services and routes

[`apps/api/src/routes/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/user-management.ts) (`b36db34b12613726b62c3d6cae7ac230991b8d97`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/user-management-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management-api.ts) (`2fd5e313c621da3b33c24c84b2805c61bb90cd99`); [`apps/web/src/user-management.css`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management.css) (`7bfafe92c470f7405b93b55e0fcc77bd71c3ad1e`); [`apps/web/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management.ts) (`105117aab66e0ef7f8a925fb760c1c2d3c8f7784`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

No matching path in this bounded inventory; semantic absence is not established.

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-settings-family-wave-company-organization-sequences.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-settings-family-wave-company-organization-sequences.md) (`a6a7f1a52e7c9b53e389f7d0ca46317c00c58884`); [`docs/governance/smp1-user-management-build-contract-reconciliation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-build-contract-reconciliation.md) (`0325b3634770f14f0dd0144908362ed554c67759`); [`docs/governance/smp1-user-management-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-certification.md) (`4c7753324e34dd202af2bb7cba123139712639ee`); [`docs/governance/smp1-user-management-deployment-gate.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-deployment-gate.md) (`9ce3619c5fd77d6535d65864f524282bc803eecb`); [`docs/governance/smp1-user-management-deployment-route-reconciliation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-deployment-route-reconciliation.md) (`fe3637a26c67fa5567a9c694d0a4b7a3d70000a4`); [`docs/governance/smp1-user-management-implementation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-implementation.md) (`3a91f110e5bd8ca1afe664a34511d679311e304d`); [`docs/governance/smp1-user-management-permission-reconciliation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-permission-reconciliation.md) (`71e3ea37a16999a3cdf94b4b236501c0c3d5afb6`); [`docs/governance/smp1-user-management-scope-lock.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-scope-lock.md) (`dec49d46d9de65cb549a08e864da101c4b31bbbe`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

The **Enterprise Human Capital, Workforce & Organizational Management Domain** is hereby formally certified as the permanent workforce architecture within the GiftHatkeOS Enterprise Canon.

This stage consolidates all previous stages into one authoritative specification governing:

- Workforce identity.
- Employee lifecycle.
- Organizational structures.
- Roles and responsibilities.
- Skills and qualifications.
- Learning and development.
- Performance management.
- Workforce planning.
- Employee experience.
- Workforce intelligence.

This certification confirms that the Human Capital Domain has achieved:

- Enterprise completeness.
- Workforce governance maturity.
- Organizational capability maturity.
- Talent development readiness.
- Privacy-aware workforce management.
- Technology independence.
- Long-term architectural stability.

---
