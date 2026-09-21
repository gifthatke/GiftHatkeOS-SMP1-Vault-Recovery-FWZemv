# Settings SMP1 Application and Transport Milestone

Status: IMPLEMENTED / NOT CERTIFIED  
Date: 2026-09-11

The shared Platform runtime now exposes the typed Settings core repository, audit writer, and all 29 family repository adapters. The API has an authenticated compatibility transport for the frozen ERP8.1 public API family plus exact ERP8.2 Company, ERP8.3 Organization, ERP8.4 Document Sequence, ERP8.5 Tax/Finance, and ERP8.6 Marketplace family waves, with CSRF protection on mutations. The web shell has the nine exact frozen Settings tabs and renders metadata-backed core registry controls.

The frozen application permission names remain exact. The active Standalone catalogue is unchanged; the compatibility bridge maps them to existing Settings permissions:

- settings.view -> settings.workspace.read
- settings.manage -> settings.configuration.update
- settings.sensitive.view -> settings.integrations.read
- settings.diagnostics.view -> settings.audit.read

The implemented family waves follow frozen fallback to existing Settings authorities; no permission #110 or User Management write wave was added.

Open gates: ERP8.7-ERP8.9 family behavior, secure secret-store provider, family UI forms, focused and cross-module tests, migration execution, Render deployment, live acceptance, certification, and closure. This is not yet live or closed.
