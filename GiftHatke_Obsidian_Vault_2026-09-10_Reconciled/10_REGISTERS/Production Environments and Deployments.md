---
title: "Production Environments and Deployments"
status: "knowledge-snapshot"
last_verified: "2026-09-10"
source: "retained-conversation-knowledge"
tags:
  - gifthatkeos
  - production
  - register
---

# Production Environments and Deployments

## Standalone
- URL: `https://erp.gifthatke.in`
- Render runtime
- Neon PostgreSQL
- Google auth
- secure HttpOnly session
- RBAC / SUPER_ADMIN
- TLS/custom domain

## Apps Script historical deployment milestones
- Version 6 — immutable baseline
- Version 7 — V1.0 Web App
- Version 8 — GLP1 PH1
- Version 18 — production at closure
- Version 19 — permanent reference freeze snapshot

## Rule
Do not confuse the frozen reference version with the live production deployment.

## September 2026 SMP1 state

- Standalone branch: `smp1/production-parity`.
- Published Shipping implementation HEAD:
  `38292121c596fc128902b0420829019fda3c58c3`.
- `https://erp.gifthatke.in` returned HTTP 200 during reachability checking.
- A root HTTP 200 is not Shipping-specific authenticated acceptance.
- Shipping migration/deployment execution was not performed in the S3A.8
  implementation wave and remains a separate closure gate.
- Procurement’s final governance closure did not require a new deployment;
  its positive production mutation acceptance and certification are complete.
