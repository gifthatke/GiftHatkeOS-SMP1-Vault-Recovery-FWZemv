# Settings SMP1 Scope Lock

Status: SCOPE LOCKED / IMPLEMENTATION ACTIVE — NOT CERTIFIED
Date: 2026-09-11

## Reconciled baseline

- Repository: `GiftHatkeOS-Standalone`
- Branch: `smp1/production-parity`
- Scope-lock commit: `e4571cf8057a2fd19467e6a5d3dc0fc96af6185b`
- Frozen repository: `GiftHatkeOS`
- Frozen HEAD: `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`
- Behavioural authority: 44-Domain Canon plus frozen Apps Script v1.0 reference.
- User Management remains certified, closed, and read-only in this wave.

## Exact Settings scope

ERP8.1–ERP8.9 configuration evidence is in scope:

- ERP8.1 core Settings registry, values, audit, 23 seeded keys, and permissions `settings.view`, `settings.manage`, `settings.sensitive.view`, `settings.diagnostics.view`.
- ERP8.2 Company profile, addresses, and bank accounts.
- ERP8.3 Organization branches, departments, and cost centres.
- ERP8.4 Document sequence registry and issuance.
- ERP8.5 GST, tax, HSN, payment terms, expense, budget, threshold, and posting configuration.
- ERP8.6 marketplace channels, SLAs, source mappings, payment methods, and fulfilment rules.
- ERP8.7 production workshops, machines, stages, priorities, QC rules, and defaults.
- ERP8.8 warehouses, storage locations, reorder, valuation, supplier, approval, goods-receipt, and units/dimensions configuration.
- ERP8.9 notification channels/rules, email, WhatsApp, escalations, reminders, templates, and communication preferences.

Frozen Settings UI evidence contains nine tabs:

1. Configuration
2. Company Profile
3. Addresses
4. Banking
5. Invoice Identity
6. Organization
7. Document Sequences
8. Tax & Finance
9. Marketplaces

## Controls

- No employee write/onboarding implementation.
- No Version 1.1, ERP9, ERP10, Domain 45, permission `#110`, architecture redesign, Reseller Dashboard, Retailer Dashboard, or Partner Dashboard.
- No invented fields, settings categories, workflows, roles, permissions, APIs, tables, migrations, routes, UI, or business rules.
- Migration necessity gate is PASS in `docs/governance/smp1-settings-migration-necessity.md`; execution remains pending deployment authorization.
- Closed modules must remain regression-safe.
- This note records scope only; it does not certify implementation, deployment, or live acceptance.

## Next milestone

Reconcile domain contracts, ports, persistence reuse, platform adapters, and exact frozen API/UI behaviour before authorizing Settings implementation code.


## Forward milestone reconciliation

- Current implementation head at milestone reconciliation: `4cc21d67d0dff65fd5295329d5058ca9014b328f`
- Migration necessity gate: PASS, recorded in `docs/governance/smp1-settings-migration-necessity.md`.
- The initial no-migration gate was respected by removing the premature artifacts; exact migrations were restored only after the separate frozen-evidence PASS.
- Settings remains not certified, not live, and not closed. ERP8.2-ERP8.4 family behavior is implemented but not certified; ERP8.5-ERP8.9 behavior, secure secret-store composition, family UI coverage, deployment, and live acceptance remain open.
