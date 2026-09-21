# User Management SMP1 Deployment Route Reconciliation

- Module: User Management
- Milestone: exact live-route reconciliation
- Branch: smp1/production-parity
- Parent head: 69cc04b387f514f9e84fafbe76b68c6e1b43940a
- Verified action: added `/users` and `/users/*` to the frozen Render static proxy route pattern.
- Persistence: no migration; existing RBAC foundation retained.
- Deployment status at record time: pending.
- Next acceptance: API deploy, static web deploy, then authenticated live ERP verification of Users, Reports, and Finance.
