# Domain 1 — Evidence Card

ORDER MANAGEMENT ENTERPRISE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-01-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `77476377-42e5-42f2-9709-ecb237fe821c`; SHA-256 `c9fbbfff7a4e729b5d06e43689cbe224fca0bd314cfce3183fe4ec663966bcd9`. Orders closure is preserved. Original Stages 1–2 add data-model requirements; schema equivalence requires field-by-field review, not table-name equality.

## Frozen Apps Script behavior

[`CERTIFICATION-v3.5.0-ERP1-ORDERS-RC1.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CERTIFICATION-v3.5.0-ERP1-ORDERS-RC1.md) (`0f1fbbb6e74d504841e448876c4800e222d26329`); [`CERTIFICATION-v3.6.1-ERP1-ORDERS-RC9.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CERTIFICATION-v3.6.1-ERP1-ORDERS-RC9.md) (`0bf511716888db4d7f4fdfb1e7ce8e5149cf7c31`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC1.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC1.txt) (`aef7ae33cc24d1d98a68ecc4d8da461c0fa67b3e`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC2.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC2.txt) (`a58a037366c454e59919c7407e9c41c4c5c15d38`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC3.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC3.txt) (`a93007b30b547dd13495158c9611e1daca7f2d7e`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC4.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC4.txt) (`faafbf4c79ad4a45750dc8aff8e9e8e89e2bbf1c`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC5.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC5.txt) (`6db7bae3fd94b3fd9e56dfbfd2fb43db1528f55d`); [`CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC8.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.5.0-ERP1-ORDERS-RC8.txt) (`1dd02971752ef77c366072620024a23b51230c55`); [`CHANGED_FILES-v3.6.1-ERP1-ORDERS-RC9.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.6.1-ERP1-ORDERS-RC9.txt) (`d91f2004f2bf8f0bad1fd3f8b455c9f0e24ba53d`); [`DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC1.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC1.md) (`56e7474ae3633e489a4922878c48d012ab081882`); [`DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC4.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC4.md) (`c16fe55cbe30ee6ae991bf53b24e44f4d38004fe`); [`DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC7.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/DEPLOYMENT-v3.5.0-ERP1-ORDERS-RC7.md) (`c9fabe9871bef6abff74a1d47b65aa7fa041f793`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/order-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/order-ports.ts) (`c8c1d0ffd8631480064993ebe16bf74855d385dd`); [`packages/domain/src/order.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/order.ts) (`fe4a9fa971537856814bb9a94812e8c9ebb87c6e`)

## Database and persistence

No matching path in this bounded inventory; semantic absence is not established.

Declared matching table candidates: order_source_mappings, order_payment_methods, order_fulfilment_rules, inventory_reorder_policies, procurement_purchase_orders, procurement_purchase_order_items, orders, order_items, order_activities, order_sequences. Schema declarations do not attest deployed data or migration success.

## Ports

[`packages/domain/src/order-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/order-ports.ts) (`c8c1d0ffd8631480064993ebe16bf74855d385dd`)

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/order.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/order.ts) (`d101feff79209c478dd91397fa24dd698d9bba2d`)

## Services and routes

[`apps/api/src/order-create-from-lead-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/order-create-from-lead-service.ts) (`3cd45daa768e3c2e9b79241ab253ad60e356495c`); [`apps/api/src/order-lookup-source-absence.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/order-lookup-source-absence.ts) (`a65d23958840a67254f1b8be566bc477f338ac3f`); [`apps/api/src/order-production-mutation-coordinator.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/order-production-mutation-coordinator.ts) (`05905cada5e5f5dc2744f04a5355046bb2a702b7`); [`apps/api/src/order-production-status-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/order-production-status-service.ts) (`8c195ea29102b9ef37d8c98730499d547009562e`); [`apps/api/src/order-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/order-service.ts) (`8572cfa59edf20dee13cb77d62d11a387aabd34f`); [`apps/api/src/production-order-synchronization-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/production-order-synchronization-service.ts) (`80201ced78b816e9c45aefcf1aca5b6b84e0c63b`); [`apps/api/src/routes/orders.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/orders.ts) (`0eb112de32044b1addcf8a92d3c874bb1b55edbb`); [`apps/api/src/shipping-order-handoff.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/shipping-order-handoff.ts) (`40242c99316a686ad7a71c43a012c9f0e7a97484`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/order-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/order-api.ts) (`35426e1198829023e8d1eeba02a4538676a5d62f`); [`apps/web/src/order-mutation-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/order-mutation-api.ts) (`7d55ec8c48fe1f8ecf88cfa00d00607947a191e1`); [`apps/web/src/orders.css`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/orders.css) (`38c89d826ddd092690dd14341effbf58039eedc7`); [`apps/web/src/orders.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/orders.ts) (`70460d7a38591ebd76ca797fcbf4376fbc5e65fd`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/order-create-from-lead-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-create-from-lead-routes.test.mjs) (`85d6dbc91443beb23f6afefce874087c9f93f135`); [`apps/api/test/order-create-from-lead-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-create-from-lead-service.test.mjs) (`3cc384bfb485ff749de4256c3759278bc08cf07f`); [`apps/api/test/order-lookup-source-absence.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-lookup-source-absence.test.mjs) (`4cc930a45cd5d46eca70a57ef995952063f35fcf`); [`apps/api/test/order-mutation-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-mutation-routes.test.mjs) (`5af420c5155fab63a9a453eacab23fef963416f7`); [`apps/api/test/order-production-mutation-coordinator.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-production-mutation-coordinator.test.mjs) (`4583af19526fa886e4b3e6454d3272f43f9e5c63`); [`apps/api/test/order-production-status-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-production-status-service.test.mjs) (`e0106e751dc02cf3b8d5623ca9b2bef5593b9784`); [`apps/api/test/order-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-routes.test.mjs) (`46b94262c1a86c2b2231b38b2c043493006e0c2e`); [`apps/api/test/order-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/order-service.test.mjs) (`ecfdbea2bea1ef9a4df4c1c18f7fe2ca051598ce`); [`apps/api/test/production-order-synchronization-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/production-order-synchronization-service.test.mjs) (`4eae4266a33c3c659f39f7e7fcbc6cbd9b288c52`); [`apps/api/test/shipping-order-handoff.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/shipping-order-handoff.test.mjs) (`2e8f8bde325766d18ca9d3af577252c9f4bd047f`); [`apps/web/test/order-mutation-workspace.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/order-mutation-workspace.test.mjs) (`d907d6ea620c590c0ce688b77a1d3578bef68dbf`); [`apps/web/test/order-read-workspace.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/order-read-workspace.test.mjs) (`8170ead566533da16aa89e679265ff2fb20e412c`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-production-order-synchronization-implementation-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-implementation-certification.md) (`5424ba72629e4c5907ede05ee547488383f4cde5`); [`docs/governance/smp1-production-order-synchronization-ownership-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-ownership-certification.md) (`158c8cf052f21b9b5ee43788c6f4ce68a8c9f359`); [`docs/governance/smp1-production-order-synchronization-ownership.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-production-order-synchronization-ownership.md) (`03ddf10101d8f3e73bb39a12c7f7dce1118c1f3f`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage represents the formal certification of the Order Domain as a permanent enterprise business capability within GiftHatkeOS.

Unlike previous stages, which define specific architectural aspects, this certification consolidates the complete Order Domain into a single authoritative business specification.

It confirms that the Order Domain has achieved enterprise-level completeness, consistency, governance, and readiness for implementation across current and future technology platforms.

This certification becomes the reference standard for all future implementations.

---
