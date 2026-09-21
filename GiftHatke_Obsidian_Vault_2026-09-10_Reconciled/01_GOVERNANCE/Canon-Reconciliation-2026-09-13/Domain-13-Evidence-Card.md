# Domain 13 — Evidence Card

INTEGRATION, APIs & ENTERPRISE CONNECTIVITY CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-13-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `28bd6b06-da6e-4662-aaa2-2c3af0c1e526`; SHA-256 `db9c4f7e25b4714b79c11299dd7143c0d0c5310f0a1972c7095eb26274bbae8a`. Current integration seams exist. Do not invent public APIs, partner portals or integrations from a domain title.

## Frozen Apps Script behavior

[`CloudinaryAssets.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CloudinaryAssets.js) (`9b2528559288cdb64575454043e5388939f37118`); [`ERP5FinanceMarketplaceGSTRegression.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP5FinanceMarketplaceGSTRegression.js) (`358ff774368ff192ac51e24496836cd2305c9c18`); [`ERP81SettingsRouterIntegration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP81SettingsRouterIntegration.js) (`c0c2062a87d4673e80929714adda1759a146e7f9`); [`ERP86MarketplaceConstants.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceConstants.js) (`6d17f82d90be66aeec2e310f5240b09dee16feb1`); [`ERP86MarketplaceIntegrationService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceIntegrationService.js) (`16386086dbb2d69bd87ed09c43859b4bc7b14e39`); [`ERP86MarketplaceModuleAdapters.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceModuleAdapters.js) (`3e679dd2aac5131570695402f3ae8b006a628a35`); [`ERP86MarketplacePermissionBridge.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplacePermissionBridge.js) (`f167dd6992d281672e7feb6150eeb5e4ea8b8159`); [`ERP86MarketplacePublicApi.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplacePublicApi.js) (`a2e91270a5ad047ad989623bec63675675f8c08a`); [`ERP86MarketplaceRepository.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceRepository.js) (`a2795f33df56277a03ef45c8c42121c58564e34c`); [`ERP86MarketplaceSchema.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceSchema.js) (`3c3faedad2c7b17c7e453c6cf67a4c84af390afd`); [`ERP86MarketplaceSeedService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceSeedService.js) (`b06b4e12f49cf03be696a95446715c511bb1f219`); [`ERP86MarketplaceService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP86MarketplaceService.js) (`825a04a5e5f88c32d3e7c2c12ad872a1ee7a9763`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/settings-marketplace-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/settings-marketplace-service.ts) (`a0b11db0a6e806bf2064daa381f808d1621e2782`)

## Database and persistence

No matching path in this bounded inventory; semantic absence is not established.

Declared matching table candidates: marketplace_channels, marketplace_slas, finance_marketplace_settlements. Schema declarations do not attest deployed data or migration success.

## Ports

No matching path in this bounded inventory; semantic absence is not established.

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/cloudinary.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/cloudinary.ts) (`d2efc68877b70ca9536f700966cdb06f0db47ee8`)

## Services and routes

[`apps/api/src/authentication/fastify-session-store.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/fastify-session-store.ts) (`1fa98c8efec2ccfd956a475e30a55f667da1b70c`); [`apps/api/src/authentication/google-auth-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-auth-config.ts) (`e692cbb7b6e672f9df4d503d9681a44632bd52bb`); [`apps/api/src/authentication/google-auth-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-auth-service.ts) (`42286defaae71268c701ec69b3aa3f77c20319d8`); [`apps/api/src/authentication/google-id-token.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/google-id-token.ts) (`7ceaaa287afb2ec344c695c7d2f30e6e0cbaf6b4`); [`apps/api/src/authentication/session-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-config.ts) (`2d4dbb65e2ddd619a469a5d3be19f588c5b5cc34`); [`apps/api/src/authentication/session-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-runtime.ts) (`6699da66ab00cceed09971692a15a3c7d18bbf3a`); [`apps/api/src/personalization-cloudinary-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/personalization-cloudinary-config.ts) (`a42c80cd3fdcedea7e2be98a069b3f2fcc25eb0e`); [`apps/api/src/personalization-cloudinary-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/personalization-cloudinary-service.ts) (`7c890694e11a8bfdfea54eb533bf0856b80744ae`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/personalization-cloudinary-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization-cloudinary-api.ts) (`e77c4098bcc3f03e0656c85498a7a2b80a79ba25`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/personalization-cloudinary-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/personalization-cloudinary-service.test.mjs) (`b7ca33be0163f0240ab92be369a90964640591cc`); [`apps/web/test/personalization-cloudinary-approval.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/personalization-cloudinary-approval.test.mjs) (`259dbcec9c7792bd2f31318a36c0e154b7a9cd1d`); [`packages/domain/test/settings-marketplace-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/settings-marketplace-service.test.mjs) (`f097094d4c0f28b829a6c9e9a92dfe4bc5d73d1b`); [`packages/platform/test/cloudinary-asset-folder.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/cloudinary-asset-folder.test.mjs) (`6f2cc4580ad28a60a3ba4347dc01707b8beae304`); [`packages/platform/test/cloudinary-resource-list-endpoint.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/cloudinary-resource-list-endpoint.test.mjs) (`714bc525004cf23a0d497d7a7243c262182ed4c1`); [`packages/platform/test/cloudinary.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/cloudinary.test.mjs) (`7f62bae9a6e0af54ec077a31f8bfb5187b50ce5a`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-settings-marketplace-wave.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-settings-marketplace-wave.md) (`7707b8969a516198ac2e1ab413eb349a6a5975d1`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage formally certifies the **Integration, APIs & Enterprise Connectivity Domain** as the permanent enterprise communication foundation within the GiftHatkeOS Enterprise Canon.

It consolidates the previous nine stages into one authoritative specification governing:

- Enterprise APIs.
- Service contracts.
- Event contracts.
- Enterprise messaging.
- Consumer and provider governance.
- Version management.
- Enterprise interoperability.
- Cross-domain synchronization.
- External connectivity.
- Communication governance.

This certification confirms that the domain has achieved:

- Enterprise completeness.
- Communication consistency.
- Governance maturity.
- Technology independence.
- Long-term architectural stability.
- Cross-domain interoperability readiness.

This document becomes the permanent reference for every future implementation of enterprise integration within GiftHatkeOS.

---
