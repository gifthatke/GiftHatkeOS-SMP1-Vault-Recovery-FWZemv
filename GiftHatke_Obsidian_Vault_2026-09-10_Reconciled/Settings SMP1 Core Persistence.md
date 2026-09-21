# Settings SMP1 Core Persistence

- Status: foundation committed; not certified/live/closed.
- Authority: frozen Apps Script ERP8.1 Settings evidence and certified Canon.
- Scope gate: core registry, values, and audit repositories only.
- Exact database mapping: `settings_registry`, `settings_values`, `settings_audit`.
- Exact migration: `20260911140000_settings_core_foundation.mjs`.
- Exact seed count: 23 frozen Settings keys.
- Value identity: `SettingKey + Environment`, preserving the frozen repository behavior.
- Persistence behavior: transaction advisory lock, optimistic version checks, inactive reset, audit append, diagnostics counts, platform-owned cache invalidation seam.
- Not included: Settings family adapters, services, routes, permissions, UI, deployment, or live acceptance.
- User Management remains read-only and closed; no employee write/onboarding work was reopened.
- Runtime build and database-backed tests are pending because the active local checkout is not mounted in this execution environment.
- Branch snapshot when milestone was prepared: `730dea28214f29d6774aee9481ae92c7d8147c28`.
