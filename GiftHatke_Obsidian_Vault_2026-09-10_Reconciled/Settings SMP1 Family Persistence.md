# Settings SMP1 Family Persistence

- Status: repository foundation committed; not certified/live/closed.
- Exact migration: `20260911143000_settings_family_foundation.mjs`.
- Exact repository count: 29 non-core tables across ERP8.2, ERP8.3, ERP8.4, ERP8.6, ERP8.8, and ERP8.9.
- Finance/Tax reuses existing certified `finance_*` tables.
- Production reuses existing certified `production_*` configuration tables.
- ERP8.8 inherited `IsDeleted`, `MetadataJson`, and `SchemaVersion` fields were preserved before adapter completion.
- Platform adapter carries transaction advisory locks, optimistic versions, archive status, soft-delete where frozen, and append-only sequence issuance.
- No Settings routes, permission assignments, UI, deployment, or live acceptance yet.
- User Management remains read-only and closed.
- Runtime build/database tests remain pending because the active local checkout is not mounted in this execution environment.
