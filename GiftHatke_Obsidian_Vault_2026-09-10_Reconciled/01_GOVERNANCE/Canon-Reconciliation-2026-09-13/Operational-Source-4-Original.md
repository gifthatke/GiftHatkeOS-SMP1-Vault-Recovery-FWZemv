---
title: "Current Operating Snapshot"
status: "current-snapshot"
last_verified: "2026-09-10"
source: "reconciled repository evidence + retained vault history"
tags:
  - gifthatke
  - current-state
  - certification
---

# Current Operating Snapshot — 10 Sep 2026

> [!warning]
> This note records the latest state retained in conversation memory. Repository evidence is authoritative if newer.

## Superseding SMP1 reconciliation — 10 Sep 2026

The August material below is retained history. The following September
reconciliation supersedes it for current SMP1 decisions.

- Branch: `smp1/production-parity`
- Current standalone HEAD: `38292121c596fc128902b0420829019fda3c58c3`
- Local, tracking and remote heads were synchronized at the reconciliation.
- Standalone worktree and index were clean.
- Frozen Apps Script reference remained at
  `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87` with a clean worktree.

### Certified and closed modules

- CRM — **CERTIFIED / LIVE / CLOSED / 100%**.
- Customers — **CERTIFIED / LIVE / CLOSED / 100%**.
- Personalization — **CERTIFIED / LIVE / CLOSED / 100%**; do not reopen without a new evidence-backed production defect.
- Procurement — **CERTIFIED / LIVE / CLOSED / 100%**. The final closure evidence reconciled 12 of 12 failure classes, with no unresolved source/runtime defect. Positive production acceptance included `MAT-000001`, `PR-000001`, `PO-000001`, `POI-000001`, `GRN-000001` and `MOV-000002`; final PO status was `Received`.

### Shipping state

- Shipping implementation scope — **100% complete and published** in commit
  `38292121c596fc128902b0420829019fda3c58c3`.
- Typecheck, build and the 47-test Shipping regression suite passed.
- The production root returned HTTP 200 during reachability checking.
- Shipping-specific authenticated production acceptance, migration/deployment
  evidence and final certification/closure were not recorded in this pass.
- Shipping must therefore not yet be labelled **CERTIFIED / LIVE / CLOSED / 100%**.

### Planned commercial partner wave

Retailer / Reseller / Partner Dashboard is a separate future Titan Lock wave.
It must not be merged into Shipping, Personalization or Procurement, and it
must not create Domain 45. The saved rules include free basic registration,
an optional adjustable ₹2,000 partner wallet/security deposit, SKU-specific
pricing, verified COGS only, target margins generally 20–35%, price-floor and
contribution-margin guards, publication statuses `READY`, `COSTS REQUIRED`,
`COST BASIS REVIEW`, `LOW MARGIN` and `NOT VIABLE`, volume-tier controls, and
explicit dropshipping, COD, shipping, returns, payments and white-label rules.

## Business
- Brand: **Gift Hatke**
- Website: **gifthatke.in**
- Tagline: **Unique gifts. hatke happiness.**
- Core business: personalized gifts for Indian consumers, marketplaces, corporate/B2B and retailers.
- Shopify is a primary commerce channel; Amazon is live; Flipkart and other marketplaces are part of the operating mix/expansion.

## Standalone production
- Production domain retained in current SMP1 work: **erp.gifthatke.in**
- Runtime foundation observed working:
  - Render production runtime
  - Neon PostgreSQL
  - Google authentication
  - secure HttpOnly session
  - production RBAC
  - SUPER_ADMIN
  - custom domain + TLS
- Orders, Production and Inventory workspaces were live and returning 200 in the latest production foundation snapshot.
- CRM later reached **CERTIFIED / LIVE / CLOSED / 100%**.

## Latest certified CRM closure retained
- Branch: `smp1/production-parity`
- Certified HEAD: `06a21606edeb606be2a0ee9ba63b77fad12f395a`
- Commit: `docs(smp1): certify CRM production parity`
- Parent/remediation: `9edc06ae959929456f05ee580d4968d9c9c1e55b`
- Certification document: `docs/governance/smp1-crm-production-certification.md`
- Certification SHA-256: `466931028f9c6413427216a657c5cf29a273498ccb003e4eb1b74c9ae7fac8da`

## Permanent locks
- 44-Domain Enterprise Canon remains authoritative.
- Frozen Apps Script GiftHatkeOS v1.0 remains behavioural reference.
- No architecture redesign during SMP1 parity work.
- No new Domain 45 unless a genuinely new business capability requires one.
- ERP9 / ERP10 and Version 1.1 remain **after** standalone v1.0 migration/certification/cutover.

## Active knowledge gaps
- Exact complete 44-domain registry is not retained in full in this snapshot.
- This vault snapshot was created on 25 Aug 2026 and does not contain every
  September repository certification artifact byte-for-byte. The repository
  remains authoritative for exact code, hashes and certification documents.
- Shipping live acceptance and final closure remain open work.
