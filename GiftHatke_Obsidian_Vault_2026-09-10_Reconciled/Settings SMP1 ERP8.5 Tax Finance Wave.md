# Settings SMP1 ERP8.5 Tax Finance Wave

Status: IMPLEMENTED — pre-deployment validation complete  
Date: 2026-09-11  
Frozen authority: `GiftHatkeOS@fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`

ERP8.5 Tax/Finance Settings now reuses the certified Finance repository set
for GST profiles, tax categories, HSN mappings, payment terms, expense and
budget categories, thresholds, and posting controls.

The exact frozen catalogue, lookups, GST/cess calculation, due-date
calculation, threshold check, future-date posting validation, administration
workspace, eight record-save families, and diagnostics are implemented.

The exact-scope gate authorized one seed-only migration for the fourteen
frozen defaults: five GST categories, four payment terms, and five expense
categories. It adds no table or column and targets only the existing Finance
tables.

Security remains contained to the existing Settings bridge:
`settings.view`, `settings.manage`, and
`settings.diagnostics.view`. Writes are CSRF protected. No new permission,
role, User Management write path, dashboard, ERP9/ERP10, Version 1.1, or
Domain 45 scope was introduced.

Focused syntax, route, seed, persistence-reuse, coupling, and forbidden-scope
gates pass. Full repository regression remains pending before deployment.
This milestone does not certify or close Settings.
