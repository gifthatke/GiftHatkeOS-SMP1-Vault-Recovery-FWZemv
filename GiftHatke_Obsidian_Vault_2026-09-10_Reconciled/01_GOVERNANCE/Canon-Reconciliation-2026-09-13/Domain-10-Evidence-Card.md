# Domain 10 — Evidence Card

DOCUMENT & DIGITAL ASSET MANAGEMENT ENTERPRISE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-10-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `c03d6185-4f57-48ba-88f4-ad20fa5706e8`; SHA-256 `36fa8838d336442289f77b217d3c9d590f92ed92a0aaa5e86fcf6be1faa0e57b`. Personalization assets and Cloudinary are bounded projections, not proof of complete document/DAM parity (GAP-012).

## Frozen Apps Script behavior

[`CloudinaryAssets.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CloudinaryAssets.js) (`9b2528559288cdb64575454043e5388939f37118`); [`ERP82DocumentBrandingIntegration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP82DocumentBrandingIntegration.js) (`7b3715c04c6b33d2398030f3697f5c1ea31cd5d6`); [`Personalization.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/Personalization.js) (`4b461934b4c5eb0e62749f80c647c2215e9c471c`); [`View_Personalization.html`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/View_Personalization.html) (`d2e23775066d387a9a4c273a2cf91a9979820218`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/personalization-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/personalization-ports.ts) (`4d63c1e7029d374c2491e3a39124d663a77736b0`); [`packages/domain/src/personalization.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/personalization.ts) (`38cf59ba2f30d37f1b18771784b284b403619bf7`)

## Database and persistence

[`packages/database/src/personalization-persistence.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/personalization-persistence.ts) (`8726a6d3a0e83ce35cc5afeb008c39aa5b64c0fe`)

Declared matching table candidates: document_sequence_registry, document_sequence_issuance, personalization_templates, personalization_intakes, personalization_assets. Schema declarations do not attest deployed data or migration success.

## Ports

[`packages/domain/src/personalization-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/personalization-ports.ts) (`4d63c1e7029d374c2491e3a39124d663a77736b0`)

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/cloudinary.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/cloudinary.ts) (`d2efc68877b70ca9536f700966cdb06f0db47ee8`); [`packages/platform/src/personalization.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/personalization.ts) (`c13cb7c8e1133333c29ade11b34efe119bb2d0c6`)

## Services and routes

[`apps/api/src/personalization-cloudinary-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/personalization-cloudinary-config.ts) (`a42c80cd3fdcedea7e2be98a069b3f2fcc25eb0e`); [`apps/api/src/personalization-cloudinary-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/personalization-cloudinary-service.ts) (`7c890694e11a8bfdfea54eb533bf0856b80744ae`); [`apps/api/src/personalization-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/personalization-service.ts) (`a37acf72787a95387aa25b8da042f25c450ac92a`); [`apps/api/src/routes/personalization.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/personalization.ts) (`69b281386397fff0049774d73e22504c7c527f2b`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/personalization-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization-api.ts) (`0bfb228eebd9311192b70c0824050391bf6aa102`); [`apps/web/src/personalization-cloudinary-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization-cloudinary-api.ts) (`e77c4098bcc3f03e0656c85498a7a2b80a79ba25`); [`apps/web/src/personalization-mutation-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization-mutation-api.ts) (`02dc10ea0db92ed10e5cb5a2ff4a044e78930ddf`); [`apps/web/src/personalization.css`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization.css) (`698de1389d3c65a9c0688763736bcd745b1aa69c`); [`apps/web/src/personalization.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/personalization.ts) (`99e418686d454b384b6468c6606a8489c368dff0`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/personalization-cloudinary-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/personalization-cloudinary-service.test.mjs) (`b7ca33be0163f0240ab92be369a90964640591cc`); [`apps/api/test/personalization-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/personalization-routes.test.mjs) (`eea5ed8f576298939d5beb2471fd6271d44168b0`); [`apps/api/test/personalization-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/personalization-service.test.mjs) (`ee561e00ce6f23d1017990e8ab686d5d84766269`); [`apps/web/test/personalization-cloudinary-approval.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/personalization-cloudinary-approval.test.mjs) (`259dbcec9c7792bd2f31318a36c0e154b7a9cd1d`); [`apps/web/test/personalization-workspace.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/personalization-workspace.test.mjs) (`7c91d26be30d6fee5e58478b7a7c823b3f8a9648`); [`packages/database/test/personalization-identity.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/personalization-identity.test.mjs) (`d9f86ec2304aa9ea5750cd5184c4d71def06a6f2`); [`packages/database/test/personalization-persistence.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/personalization-persistence.test.mjs) (`3a268e040d704e0291f480eb1ab8af536aaeca4c`); [`packages/database/test/personalization-repository.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/personalization-repository.test.mjs) (`c86d049efd5ac0b1f9fd926a531f3bc805c84617`); [`packages/domain/test/personalization-ports.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/personalization-ports.test.mjs) (`3302490062989243c77175d856336a04e2e9f7dc`); [`packages/domain/test/personalization.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/test/personalization.test.mjs) (`b1f353b8f2eea1a2e2d5cf8a6175ede6fd4c0ee2`); [`packages/platform/test/cloudinary-asset-folder.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/cloudinary-asset-folder.test.mjs) (`6f2cc4580ad28a60a3ba4347dc01707b8beae304`); [`packages/platform/test/cloudinary-resource-list-endpoint.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/test/cloudinary-resource-list-endpoint.test.mjs) (`714bc525004cf23a0d497d7a7243c262182ed4c1`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-personalization-parity-contract-lock.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-personalization-parity-contract-lock.md) (`3bb3374153e2d4796c36f50433426cfae020ac86`); [`docs/governance/smp1-personalization-production-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-personalization-production-certification.md) (`c1c6144e1826a3836428f51c59bf315de12f6cdb`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage formally certifies the **Document & Digital Asset Management Domain** as the permanent enterprise information foundation within the GiftHatkeOS Enterprise Canon.

It consolidates the previous nine stages into one authoritative specification governing:

- Enterprise documents.
- Digital assets.
- Engineering documentation.
- Customer personalization assets.
- Enterprise knowledge.
- Version governance.
- Metadata governance.
- Retention governance.
- Archive governance.
- Information intelligence.
- Enterprise information stewardship.

This certification confirms that the domain has achieved:

- Enterprise completeness.
- Information consistency.
- Governance maturity.
- Technology independence.
- Long-term architectural stability.
- Cross-domain integration readiness.

This document becomes the permanent reference for every future implementation of enterprise information management within GiftHatkeOS.

---
