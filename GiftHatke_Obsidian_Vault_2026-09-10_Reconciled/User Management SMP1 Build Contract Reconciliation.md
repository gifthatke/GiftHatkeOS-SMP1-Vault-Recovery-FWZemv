# User Management SMP1 Build Contract Reconciliation

- Milestone: Render API build contract
- Branch: smp1/production-parity
- Corrected head: b86915b079cede5bc0686ed46145bd49dbe004d0
- Observed failure: selected database rows were typed as table definitions rather than Kysely selectable rows.
- Corrected files: `packages/platform/src/user-management.ts`, `packages/platform/src/user-management-lifecycle.ts`.
- No migration or new database table.
- Deployment remains pending until the exact Render build succeeds.
