# Settings SMP1 Migration Necessity Gate

Status: PASS — exact frozen evidence proves physical storage is required  
Date: 2026-09-11

The scope lock deferred migrations pending evidence. Frozen GiftHatkeOS at fd7c754fb1be380e6d3f9b01dd041b97b82f1d87 exposes the ERP8.1 registry/value/audit repositories and the ERP8.2–ERP8.9 configuration repository families. The Standalone closure baseline had no PostgreSQL Settings tables, so typed adapters cannot provide production parity without physical storage.

The authorized migration set is exactly the three core tables plus the 29 already scope-locked family tables. No new Settings category, permission, User Management write surface, dashboard, ERP9/ERP10, Version 1.1, or Domain 45 is included.

An earlier migration trail was corrected forward-only: the unapproved files were removed, this gate was recorded, and the exact migration files are restored only after this PASS. Git history remains immutable and the frozen repository remains untouched.

This gate authorizes only the migration wave; it does not certify, deploy, or close Settings.

## ERP8.5 seed-DML addendum

The exact-scope migration gate was re-run against frozen
`ERP85TaxFinanceSeedService.js`. It authorizes seed-only, idempotent DML for
five GST tax categories, four payment terms, and five expense categories in
the already-certified Finance tables. It authorizes no schema, permission,
route, workflow, dashboard, or Finance workspace change.

Result: PASS — ERP8.5 exact seed-only DML may proceed.

## ERP8.7 seed-DML addendum

The gate was re-run against frozen ERP8.7 repository and installer evidence.
It authorizes exactly sixteen idempotent defaults: one workshop, one machine,
six stages, four priorities, and four QC rules in existing Settings family
tables. No Production default row is part of the frozen seed.

Result: PASS — seed-only DML; no schema, operational Production workflow,
permission, or closed Production UI change.

## ERP8.8 seed-DML addendum

Frozen ERP8.8 repository evidence requires exactly twelve defaults across the
eight existing Inventory/Procurement configuration tables: seven single
configuration records and five units/dimensions.

Result: PASS — seed-only DML; no schema, operational Inventory/Procurement
workflow, permission, or closed-module UI change.

## ERP8.9 seed-DML addendum

Frozen ERP8.9 repository evidence requires exactly ten defaults across eight
existing Notification/Communication configuration tables: three channels and
seven single-family records.

Result: PASS — seed-only DML; no schema, outbound dispatch, provider
credential, permission, or operational notification workflow.
