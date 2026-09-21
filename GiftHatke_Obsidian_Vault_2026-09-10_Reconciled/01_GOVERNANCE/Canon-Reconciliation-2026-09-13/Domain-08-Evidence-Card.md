# Domain 8 — Evidence Card

IDENTITY, SECURITY & ADMINISTRATION ENTERPRISE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-08-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `d018a383-672b-4657-968b-0f05531c2af4`; SHA-256 `a004488b7129c190cc123e20ebdbd9d04dc6038d0185f323aa857ee438d3bc36`. GAP-003 remains open. User workspace stays read-only; role/assignment UI is a potential later wave. Invitations/onboarding are not established requirements.

## Frozen Apps Script behavior

[`AuthorizationService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/AuthorizationService.js) (`9aa70382201b913a43caedbf178ad5c37d7e02ba`); [`ERP73PermissionMatrixApi.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73PermissionMatrixApi.js) (`e0f7a499d935c66e3bda33277a900e76642c050f`); [`ERP73PermissionMatrixIntegration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73PermissionMatrixIntegration.js) (`0812fab46d32fea9fc102cbba8de36f49d7ec700`); [`ERP73PermissionMatrixService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73PermissionMatrixService.js) (`ad476685792aa0771e985e7ca0517b8bb53b10a6`); [`ERP73PermissionMatrixTests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73PermissionMatrixTests.js) (`eaf6259227a5f8faa7857a8abf62a6c62e560093`); [`ERP73RoleAnalyticsApi.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleAnalyticsApi.js) (`ec905c185b37d99c76a0db3718a156f839c1b6da`); [`ERP73RoleAnalyticsService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleAnalyticsService.js) (`4e85274756fca9938b0a8921acc0a14c386a2fd4`); [`ERP73RoleApiContracts.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiContracts.js) (`9dc55b8a084b7d5bc4c04a7719fefeae096f63e6`); [`ERP73RoleApiSprint31Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint31Integration.js) (`dc7e360de23287a8e0b8f53ed6e06d4b5a5d5f5d`); [`ERP73RoleApiSprint31Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint31Tests.js) (`27a6c2e369dd56a7376265843be258f50c207da1`); [`ERP73RoleApiSprint32Integration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint32Integration.js) (`d42a3f113e06d48dcacc2dd938d342883e8086e8`); [`ERP73RoleApiSprint32Tests.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleApiSprint32Tests.js) (`9468db03a25f0c7a79b983a83cec2a90040e4341`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/permission-catalogue.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/permission-catalogue.ts) (`dc7f05f69c3e8dc27689f2281db12b4db60ea41a`); [`packages/domain/src/security.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/security.ts) (`93bdebd3330786f98bfb28225f9c4ecb2c3e3776`); [`packages/domain/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/user-management.ts) (`18368656833ef5e338d561fb5112ef7afda04653`)

## Database and persistence

No matching path in this bounded inventory; semantic absence is not established.

Declared matching table candidates: auth_sessions, roles, role_permissions, user_role_assignments, security_audit. Schema declarations do not attest deployed data or migration success.

## Ports

No matching path in this bounded inventory; semantic absence is not established.

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/security-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/security-runtime.ts) (`8fe2fc8be292338d32f43c230555d81805806779`); [`packages/platform/src/security.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/security.ts) (`5c4d3dabb7b2eb04d58ba8da9cd45a4d1bfd3cb2`); [`packages/platform/src/user-management-lifecycle.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/user-management-lifecycle.ts) (`bd45f5403d249aad521d68b3efc9884e71b07a30`); [`packages/platform/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/user-management.ts) (`bda6751dbf24673333c2eb37aa1a7c50c16f42b7`)

## Services and routes

[`apps/api/src/authentication/fastify-session-store.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/fastify-session-store.ts) (`1fa98c8efec2ccfd956a475e30a55f667da1b70c`); [`apps/api/src/authentication/google-auth-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-auth-config.ts) (`e692cbb7b6e672f9df4d503d9681a44632bd52bb`); [`apps/api/src/authentication/google-auth-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-auth-service.ts) (`42286defaae71268c701ec69b3aa3f77c20319d8`); [`apps/api/src/authentication/google-id-token.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-id-token.ts) (`7ceaaa287afb2ec344c695c7d2f30e6e0cbaf6b4`); [`apps/api/src/authentication/session-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-config.ts) (`2d4dbb65e2ddd619a469a5d3be19f588c5b5cc34`); [`apps/api/src/authentication/session-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-runtime.ts) (`6699da66ab00cceed09971692a15a3c7d18bbf3a`); [`apps/api/src/routes/auth-session.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/auth-session.ts) (`e6742641b516b47117ecb924b668a2d0df665893`); [`apps/api/src/routes/security-access.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/security-access.ts) (`6bf21d63ab330780598b238699554bf9bf26731a`); [`apps/api/src/routes/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/user-management.ts) (`b36db34b12613726b62c3d6cae7ac230991b8d97`); [`apps/api/src/security.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/security.ts) (`c8cc3ee39ad0fa9fcb6ca16c0956fbcf485d9797`); [`apps/api/src/settings-authorization.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/settings-authorization.ts) (`f055b080590aac2bbff595e0959423496f82f38b`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/auth.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/auth.ts) (`38de84bc06894adce2a2bca57aa52641f0cf719d`); [`apps/web/src/user-management-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management-api.ts) (`2fd5e313c621da3b33c24c84b2805c61bb90cd99`); [`apps/web/src/user-management.css`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management.css) (`7bfafe92c470f7405b93b55e0fcc77bd71c3ad1e`); [`apps/web/src/user-management.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/user-management.ts) (`105117aab66e0ef7f8a925fb760c1c2d3c8f7784`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/auth-session-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/auth-session-routes.test.mjs) (`af151f36f11e734add5d4505674c24eef6e97c8c`); [`apps/api/test/google-auth-config.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/google-auth-config.test.mjs) (`0bc6d8a06455ca6ebbcd57188b38410e4a143656`); [`apps/api/test/request-authorization.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/request-authorization.test.mjs) (`0376b85a64c6cf40428975e84c018ff65cfece4a`); [`apps/api/test/security-access-route.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/security-access-route.test.mjs) (`76068f44610dea69d06b026a3255ffd2ef1a9d98`); [`apps/api/test/security-error-boundary.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/security-error-boundary.test.mjs) (`c30e7031b6177547dd70cd5ec89ea03f6ec1a56f`); [`apps/web/test/auth-login-visibility.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/auth-login-visibility.test.mjs) (`6b11244f173922bb1c9ef1c06fc9becd7deb9bb3`); [`packages/domain/test/permission-catalogue.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/permission-catalogue.test.mjs) (`e2a8ac9c008da8661287f89fd85a6ad48f9b8aef`); [`packages/domain/test/security.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/security.test.mjs) (`944a4bb645f5fc4117d736b9b4c52fc917916328`); [`packages/platform/test/security-runtime.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/security-runtime.test.mjs) (`7e946203c210197e3b70777739ab4e0f7cda0448`); [`packages/platform/test/security.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/security.test.mjs) (`298c0e067d4f74d8430fceab52628e55c19ee2a6`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-inventory-procurement-mutation-permission-mapping.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-procurement-mutation-permission-mapping.md) (`4961430f7eb32c1711580f81dd2284df005c5186`); [`docs/governance/smp1-procurement-production-auth-security-acceptance-scope-lock.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-procurement-production-auth-security-acceptance-scope-lock.md) (`b6dcfd0e40d183b33900833d52dddce993c14672`); [`docs/governance/smp1-shipping-s3a7-a2-owner-mapping-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-a2-owner-mapping-authorization.md) (`90040f3fe2b100684bba29eba0b2726281f5dc6c`); [`docs/governance/smp1-shipping-s3a7-c-backend-infrastructure-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-c-backend-infrastructure-authorization.md) (`a8750381c55723abb17e4c2003c6c70071cc332b`); [`docs/governance/smp1-shipping-s3a7-d-server-composition-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-d-server-composition-authorization.md) (`de3cd1d3219f84c47892d7b29dc42af74d5c199a`); [`docs/governance/smp1-shipping-s3a7-server-composition-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-server-composition-authorization.md) (`659686c1c77d566bed2e51360b8a0d22d3f4c317`); [`docs/governance/smp1-shipping-s3a8-employee-workspace-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a8-employee-workspace-authorization.md) (`959aa4a342e113fc94496a9655e4355e42003140`); [`docs/governance/smp1-user-management-build-contract-reconciliation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-user-management-build-contract-reconciliation.md) (`0325b3634770f14f0dd0144908362ed554c67759`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage formally certifies the **Identity, Security & Administration Domain** as the permanent enterprise governance foundation within the GiftHatkeOS Enterprise Canon.

It consolidates the previous nine stages into one authoritative specification governing:

- Enterprise identities.
- Organizational structure.
- Authorization.
- Roles.
- Permissions.
- Delegation.
- Administrative governance.
- Security policies.
- Enterprise administration.
- Cross-domain authorization.

This certification confirms that the domain has achieved:

- Enterprise completeness.
- Governance consistency.
- Technology independence.
- Long-term architectural stability.
- Cross-domain integration readiness.

This document becomes the permanent reference for every future implementation of enterprise identity, authorization, administration, and governance within GiftHatkeOS.

---
