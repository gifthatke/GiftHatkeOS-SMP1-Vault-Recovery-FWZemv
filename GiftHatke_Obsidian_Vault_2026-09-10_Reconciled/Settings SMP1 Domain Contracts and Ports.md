# Settings SMP1 Domain Contracts and Ports

Status: IMPLEMENTATION WAVE READY / RUNTIME BUILD PENDING
Date: 2026-09-11

## Reconciliation

- Scope-lock commit: `e4571cf8057a2fd19467e6a5d3dc0fc96af6185b`
- Domain/ports milestone: `5791f82ad6e385ae104f7ad955f9131fc7e2e114`
- Branch: `smp1/production-parity`
- Frozen HEAD: `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`

## Delivered

- Technology-independent Settings domain contracts covering the exact ERP8.1–ERP8.9 repositories.
- Exact 23 core setting keys, four Settings permissions, nine UI tabs, and frozen repository header maps.
- Version-aware canonical repository port plus the complete repository-set contract.
- Focused contract and port tests.
- No persistence, migration, service, route, UI, permission registration, or live-data mutation.

## Control

A tax-model mapping error was caught and corrected before the final milestone: GST uses `TaxGstConfiguration`, tax categories use `TaxCategory`, and budget categories use `FinanceBudgetCategory`.

Runtime build/test execution remains pending because the local Mac checkout is not mounted in the current cloud runtime and this branch has no configured GitHub Actions workflow. The next wave is persistence reuse and schema/migration reconciliation.
