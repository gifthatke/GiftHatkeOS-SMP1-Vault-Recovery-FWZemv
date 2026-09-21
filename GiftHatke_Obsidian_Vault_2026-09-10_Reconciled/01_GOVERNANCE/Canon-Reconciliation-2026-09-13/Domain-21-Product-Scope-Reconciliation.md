# Domain 21 — Product Scope Reconciliation

**Wave:** SMP1 Canon Recovery and Final Reconciliation (governance-only)  
**Classification:** **partial foundation**  
**Decision:** no Product Master, catalog, merchandising, or SKU implementation is authorized in this wave.

## Canon requirement

The recovered exact source is `Domain-21-Stage-10-Original.md`, titled **Enterprise Product Lifecycle, Catalog & Merchandising Management Canon — Stage 10 — Final Enterprise Product Certification**. The source message is `89b75e7c-ef7b-4768-b10f-94b722b6ac4f`; its SHA-256 is `6ddb9de0dfb0192cde2f06bfb80f5d97e91e2271cef4e8bafb550fcc47f1f355`. It certifies product identity, product families/categories, variants, SKUs, specifications, configuration, personalization rules, templates, catalogs, merchandising, lifecycle, intelligence, audit, and migration assurance. The certified lifecycle is Concept → Defined → Validated → Approved → Active → Modified → Retired → Archived.

## Frozen reference comparison

The frozen repository at `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87` contains production and inventory reference material, including `Production.js`, `ProductionService.js`, ERP3 inventory certification/regression records, and production/inventory schema and service files. Exhaustive frozen-tree filename search found no dedicated Product Master, Catalog, SKU, or Merchandising implementation. The frozen records therefore provide adjacent production and inventory behavior, not proof of the complete Domain 21 capability set.

## Active repository comparison

The active repository at `06090ddcd8a68c6be6ef484d43b252547bf5ec55` contains inventory/procurement, personalization, production, and order-product port seams. It has no dedicated Product domain package, product persistence model, product adapter, product routes, or product workspace. Existing personalization and inventory structures are related capabilities and cannot be treated as the Canon Product Master. The order-product lookup seam remains explicitly source-absent in the current server composition.

## Persistence, ports, adapters, and acceptance

The indexed inventory, personalization, and production tables and services cover adjacent operational concerns. They do not establish the certified Product model, lifecycle, catalog, merchandising, SKU identity, or product audit history. No fresh authenticated live ERP acceptance is available for Domain 21: the 2026-09-13 cloud check reached the ERP sign-in screen, while the Google sign-in popup returned HTTP 502. Historical acceptance evidence is retained separately and is not a substitute for current acceptance.

## Reconciliation decision

Domain 21 remains **partial foundation**. Existing findings remain preserved, including GAP-012 (partial foundation) and GAP-013 (post-SMP1 / ERP9 / ERP10 / Version 1.1). This note does not create a new implementation requirement, reopen a closed module, or change the User Management or Settings decisions. A future Product-authorized wave would require explicit authority evidence and a complete Product parity plan before any code or deployment work.

## Provenance

See [[Domain-21-Evidence-Card]], [[SMP1-44-Domain-Parity-Working-Matrix]], and `Domain-21-Product-Source-Provenance.json`.
