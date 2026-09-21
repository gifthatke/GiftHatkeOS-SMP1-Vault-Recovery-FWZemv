# User Management SMP1 Scope Lock

Status: SCOPE LOCKED / IMPLEMENTATION AUTHORIZED
Module: User Management
Scope: ERP7.1 user management, ERP7.2 permissions, ERP7.3 role management, and ERP7.4 user–role assignment.

## Reconciled authority

- Standalone baseline: 842e155b440ff3ee328058754381e586999921f2
- Frozen Apps Script reference: fd7c754fb1be380e6d3f9b01dd041b97b82f1d87
- Active branch: smp1/production-parity
- Live ERP: https://erp.gifthatke.in
- Reports and Finance remain certified/live/closed; the Reports 8/7/1/88 provider warning is the documented frozen outcome, not an ERP application error.

## Locked contract

The implementation is limited to the frozen ERP7.1–ERP7.4 public operations:

- user workspace/session and user save behavior;
- permission catalogue, current context, menu/security validation, role matrix, diagnostics, and cache invalidation;
- role workspace, search/details/hierarchy/templates/diagnostics, effective permissions, and create/update/clone/archive/restore/activate/deactivate;
- assignment create/update/set-primary/revoke/restore/effective/list/diagnostics/cache invalidation.

The exact frozen user, role, permission, assignment, audit, validation, status, and lifecycle fields remain authoritative. Standalone transport is only a projection of that public surface.

## Persistence decision

Reuse the current Standalone RBAC tables and security audit persistence:

- users;
- roles;
- role_permissions;
- user_role_assignments;
- security_audit.

No User Management table or migration is authorized.

## Explicit exclusions

No Reseller/Retailer/Partner Dashboard, Version 1.1, ERP9, ERP10, Domain 45, redesign, frozen-reference mutation, or non-evidenced capability is in scope.

## Gate status

Scope/contracts: LOCKED.
Ports: pending.
Persistence: reuse/no migration, pending proof.
Platform: pending.
Migrations: no migration, pending gate.
Services/routes: pending.
Workspace/UI: pending.
Regression: pending.
Deployment/live acceptance: pending.
Closure: pending.

This record is reconciled at scope-lock milestone; it must be updated after each later milestone and before closure.
