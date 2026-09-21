# Settings SMP1 Sensitive Provider Disposition

Date: 2026-09-12
Status: Formally classified; new Titan Lock wave required
Implementation evidence: `ffde4c20c15f1630dfc822a073cb94d82b504acf`
Governance record: `docs/governance/smp1-settings-sensitive-provider-disposition.md`

Frozen Apps Script stores protected Settings values through `PropertiesService` and persists only secret references. Standalone has the provider-neutral secret-store port and masking/fail-closed boundaries, but its production composition intentionally uses `createUnavailableSettingsSecretStore()`.

Classification: **missing and requiring a new Titan Lock wave**. New or changed banking account numbers and other sensitive Settings values cannot currently be stored or revealed. Non-sensitive Settings behavior remains live. No secret was entered during acceptance, and no plaintext secret is present in PostgreSQL, Render configuration, logs, source, or governance evidence.

The current Settings wave can close under its explicit deployment gate only with this disposition carried forward. It is not a declaration of end-to-end sensitive-write parity. Provider selection, credentials, key lifecycle, adapter composition, recovery, and live acceptance require separate authority.
