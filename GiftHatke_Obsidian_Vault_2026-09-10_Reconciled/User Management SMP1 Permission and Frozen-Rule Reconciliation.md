# User Management SMP1 Permission and Frozen-Rule Reconciliation

- Date: 2026-09-11
- Branch: smp1/production-parity
- Head: 329b935c740d2694097e40e5f71210c3a00d5214
- Status: IMPLEMENTED / RULE RECONCILED / DEPLOYMENT PENDING

Invalid non-catalogue all permission keys were removed from the User Management transport and audit projections. Routes now use the frozen ERP7.2 granular action keys. ERP7.3 role type and protected/system lifecycle validation now matches the frozen reference. No new table or migration was added.
