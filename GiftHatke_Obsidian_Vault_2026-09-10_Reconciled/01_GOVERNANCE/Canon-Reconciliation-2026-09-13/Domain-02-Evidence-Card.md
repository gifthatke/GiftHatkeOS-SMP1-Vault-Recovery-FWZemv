# Domain 2 — Evidence Card

INVENTORY & PROCUREMENT ENTERPRISE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-02-Stage-10-Original]]; original certification state: **Certification Status:** ✅ **CERTIFIED**. Message `d28a6fbf-3b79-419f-b65f-8adcd47656e7`; SHA-256 `9ca55ea221e5b567ff645ff19facca69d5e5fa6c24f63fdbfb898dbffb6c2a45`. Inventory and Procurement remain separate implementation modules inside the single exact Canon Domain 2; no domain split.

## Frozen Apps Script behavior

[`CERTIFICATION-v3.6.3-ERP3-INVENTORY-RC1.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CERTIFICATION-v3.6.3-ERP3-INVENTORY-RC1.md) (`4c07c4943bad3b2c65c1c9ba5c40b87fabfb3216`); [`CHANGED_FILES-v3.6.3-ERP3-INVENTORY-RC1.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHANGED_FILES-v3.6.3-ERP3-INVENTORY-RC1.txt) (`dd717a4b965ed2e2f5d81615510d428fe5bcde75`); [`CHECKSUMS-v3.9.10-GLP1-INVENTORY-MATERIAL-UX-HOTFIX2.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHECKSUMS-v3.9.10-GLP1-INVENTORY-MATERIAL-UX-HOTFIX2.txt) (`7f1683862b34af94957a1ecfc115f2117afe5209`); [`CHECKSUMS-v3.9.10-GLP1-INVENTORY-REQUISITION-RECONCILIATION-HOTFIX1.txt`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CHECKSUMS-v3.9.10-GLP1-INVENTORY-REQUISITION-RECONCILIATION-HOTFIX1.txt) (`a19f4449576c23ec986035004e28efe0f724ed13`); [`CONFIGURATION_INVENTORY.json`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/CONFIGURATION_INVENTORY.json) (`d2a67d4bc12e7258b4a1ad3038ff2f988d733a4b`); [`DEPLOYMENT-v3.6.3-ERP3-INVENTORY-RC1.md`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/DEPLOYMENT-v3.6.3-ERP3-INVENTORY-RC1.md) (`968dee68be8f81716174f53a5f2b38ebdb4710d7`); [`ERP3InventoryRegression.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP3InventoryRegression.js) (`01690886f79e7db47f03745142bc0578d96f1c29`); [`ERP88InventoryProcurementAudit.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP88InventoryProcurementAudit.gs) (`f8ae429baae429614f771f7c80cfc28fe016d4ea`); [`ERP88InventoryProcurementBackupService.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP88InventoryProcurementBackupService.gs) (`21e14085c75af5b2c574e37aa363ff13dae87587`); [`ERP88InventoryProcurementCache.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP88InventoryProcurementCache.gs) (`9e0487acf348f3ce66202424046460d42e937eb0`); [`ERP88InventoryProcurementCertificationService.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP88InventoryProcurementCertificationService.gs) (`93ff3d05c54612beec822f5c90ecde65df5fa49f`); [`ERP88InventoryProcurementConstants.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP88InventoryProcurementConstants.gs) (`e89c64de64ab9e37cf6c52332b7a9a114e9b2cd4`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/inventory-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/inventory-ports.ts) (`ba2d26af4844919de676a44599d0b7d46b5e823b`); [`packages/domain/src/inventory.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/inventory.ts) (`9e9193dc5fa98f58a2f473ff1015f8e99f0ced38`); [`packages/domain/src/procurement-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/procurement-ports.ts) (`bafb00fb9d51870507380d0f58036c8300d2c02c`); [`packages/domain/src/procurement.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/procurement.ts) (`3c614ab7fbb7e91fe0ae7b4940025e527d17bd81`); [`packages/domain/src/settings-inventory-procurement-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/settings-inventory-procurement-service.ts) (`93b618aac6b6005f4388dd52adfb8f4d699ce975`)

## Database and persistence

[`packages/database/src/inventory-procurement-identity.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/inventory-procurement-identity.ts) (`e1c538a5f92f57f3d4a892dbaa063ecc3dfac055`); [`packages/database/src/inventory-procurement-persistence.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/inventory-procurement-persistence.ts) (`a38ff1c4940f6a9211d26f7b98a055fab4b9de6f`)

Declared matching table candidates: inventory_warehouses, inventory_storage_locations, inventory_reorder_policies, inventory_valuation_preferences, procurement_supplier_rules, procurement_approval_thresholds, procurement_goods_receipt_defaults, inventory_units_dimensions, inventory_material_audit, inventory_material_activities, inventory_materials, inventory_movements, inventory_bom_lines, procurement_purchase_requisitions, procurement_purchase_orders, procurement_purchase_order_items, procurement_goods_receipts, procurement_goods_receipt_items, procurement_timeline. Schema declarations do not attest deployed data or migration success.

## Ports

[`packages/domain/src/inventory-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/inventory-ports.ts) (`ba2d26af4844919de676a44599d0b7d46b5e823b`); [`packages/domain/src/procurement-ports.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/procurement-ports.ts) (`bafb00fb9d51870507380d0f58036c8300d2c02c`)

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/inventory-procurement-identity.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/inventory-procurement-identity.ts) (`9609194b6bdbdb131bade3823ef2f738b6f0b234`); [`packages/platform/src/inventory-procurement.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/inventory-procurement.ts) (`bb763e99e454239a623086d92ceda40273e9b7da`)

## Services and routes

[`apps/api/src/inventory-material-requisition-refresh.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/inventory-material-requisition-refresh.ts) (`4a9e01f119aa5f428cc1ebaee4a7296415de6f0c`); [`apps/api/src/inventory-material-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/inventory-material-service.ts) (`e9cfd72ad102ee44814f75226c0439204f2b83af`); [`apps/api/src/inventory-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/inventory-service.ts) (`e1f8b02b408c20de850f323933354233adcaf6bf`); [`apps/api/src/procurement-grn-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/procurement-grn-service.ts) (`0c3c7163c0d7b8a9399131ed4ad2cb7b278fb60c`); [`apps/api/src/procurement-po-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/procurement-po-service.ts) (`76c61864e9c0e98ddd781592fb7d2d1ccf02b696`); [`apps/api/src/procurement-pr-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/procurement-pr-service.ts) (`907ad1300f21766a76137bd0fbf7c098f186dcb7`); [`apps/api/src/production-inventory-consumption-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/production-inventory-consumption-service.ts) (`f335dfda2bb40b56ad5fb1b69f0a147294261ff6`); [`apps/api/src/routes/inventory-materials.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/inventory-materials.ts) (`4c2892860a2ba441c68a91e7f76165a56e860a0b`); [`apps/api/src/routes/inventory-procurement.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/inventory-procurement.ts) (`5a121297207be67627772884009baa0ddd5b79a8`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

[`apps/web/src/inventory-procurement-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/inventory-procurement-api.ts) (`e593186f40ad051f3e54337e27f28cd7e62ee786`); [`apps/web/src/inventory-procurement-mutation-api.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/inventory-procurement-mutation-api.ts) (`e3101cad329deef77e83d1accddd17f8511c36da`); [`apps/web/src/inventory-procurement.css`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/inventory-procurement.css) (`7eec9f1e04253b8286726147d573d500b0e1ff3f`); [`apps/web/src/inventory-procurement.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/src/inventory-procurement.ts) (`05ad1699ec28ab052508755f29410bd95cf8582a`)

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/inventory-material-effects.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-material-effects.test.mjs) (`4c558f29ad999c964d46a3f69d94a81de02e3907`); [`apps/api/test/inventory-material-http.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-material-http.test.mjs) (`9373f4f6497ade0e01401ed23c7104115905b521`); [`apps/api/test/inventory-material-pr-transaction-scope-additive.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-material-pr-transaction-scope-additive.test.mjs) (`e0402d763655a73d3d954a8706c34e8b5654d9a1`); [`apps/api/test/inventory-material-requisition-refresh.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-material-requisition-refresh.test.mjs) (`4934b72bc7a0ebefa29347cd6a12782d8f4010fc`); [`apps/api/test/inventory-material-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-material-service.test.mjs) (`e9342c30ad2391481a9af69bf80ca0a0505ee230`); [`apps/api/test/inventory-procurement-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-procurement-routes.test.mjs) (`73ce1d289dd15aa3c0ab43c20017707c1893e75b`); [`apps/api/test/inventory-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/inventory-service.test.mjs) (`e23a2bff11d8e371c62581734f7f00f7f91071ae`); [`apps/api/test/procurement-grn-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/procurement-grn-service.test.mjs) (`3fc691a2798f24895de51e65921f07375e10274f`); [`apps/api/test/procurement-po-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/procurement-po-service.test.mjs) (`0ebf44ddf77724694bb30e1335e1babad1579d44`); [`apps/api/test/procurement-pr-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/procurement-pr-service.test.mjs) (`ffb3a8d024c40c7d7815357d416ea8958c151590`); [`apps/api/test/production-inventory-consumption-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/production-inventory-consumption-service.test.mjs) (`517a6d907d39cb07b019b552a562e17127dd76fd`); [`apps/web/test/inventory-procurement-mutation-workspace.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/web/test/inventory-procurement-mutation-workspace.test.mjs) (`a3fc5f010cc7fce740a6016a888533e5e38d06b4`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-inventory-material-identity-implementation-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-identity-implementation-certification.md) (`9e6f447ead1db04d74bd662f206d14d3a749a18e`); [`docs/governance/smp1-inventory-material-identity-ownership.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-identity-ownership.md) (`eb0a9b533cbfb73568f034d5520460291c806210`); [`docs/governance/smp1-inventory-material-mutation-application-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-mutation-application-certification.md) (`03b1a63d517fa381681bf19167b068811df8b4ee`); [`docs/governance/smp1-inventory-material-mutation-effects-ownership-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-mutation-effects-ownership-certification.md) (`885e99d257ef4a32c72f16d85e3852856d70a5d1`); [`docs/governance/smp1-inventory-material-mutation-effects-ownership.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-mutation-effects-ownership.md) (`a05312ed3440984b32c2f32c83de6d3fe8994628`); [`docs/governance/smp1-inventory-material-mutation-http-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-material-mutation-http-certification.md) (`c20dab1fe4ac4e2a4391dd3a182f8d48f917443b`); [`docs/governance/smp1-inventory-procurement-mutation-permission-mapping.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-inventory-procurement-mutation-permission-mapping.md) (`4961430f7eb32c1711580f81dd2284df005c5186`); [`docs/governance/smp1-material-requisition-refresh-behavior-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-material-requisition-refresh-behavior-certification.md) (`510e755ff609f84158e637fe0e05d4f83e370c16`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

This stage formally certifies the **Inventory & Procurement Domain** as a permanent enterprise business capability within the GiftHatkeOS Enterprise Canon.

It consolidates the previous nine stages into a single authoritative specification for inventory governance, warehouse operations, procurement management, and supplier coordination.

This certification confirms that the Inventory & Procurement Domain has achieved:

- Enterprise completeness.
- Operational consistency.
- Business governance.
- Technology independence.
- Long-term architectural stability.

This document becomes the permanent reference for every future implementation of inventory and procurement within GiftHatkeOS.

---
