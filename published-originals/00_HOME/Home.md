---
title: "Gift Hatke — Knowledge Home"
status: "knowledge-snapshot"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatke
  - moc
  - home
---

# Gift Hatke — Knowledge Home

## Executive navigation
- [[Current Operating Snapshot]]
- [[../01_GOVERNANCE/TITAN LOCK - Master Governance]]
- [[../01_GOVERNANCE/Authority Hierarchy]]
- [[../01_GOVERNANCE/Enterprise Canon - 44 Domain Overview]]
- [[../01_GOVERNANCE/Roadmap - Locked Sequence]]
- [[../02_BUSINESS/Business MOC]]
- [[../03_PRODUCTS/Product MOC]]
- [[../04_COMMERCE_MARKETING/Commerce and Marketing MOC]]
- [[../05_OPERATIONS/Operations MOC]]
- [[../07_GIFTHATKEOS/00_MOC/GiftHatkeOS MOC]]
- [[../08_KNOWLEDGE_BASE/Knowledge Base Charter]]
- [[../09_TIMELINE/Gift Hatke Timeline]]
- [[../10_REGISTERS/Decision Register]]
- [[../10_REGISTERS/Branches Commits Tags and Certifications]]
- [[../10_REGISTERS/Open Issues Risks and Unknowns]]

## Knowledge graph
```mermaid
flowchart TD
  B[Gift Hatke Business] --> C[Commerce]
  B --> O[Operations]
  B --> P[Products]
  O --> OS[GiftHatkeOS]
  C --> OS
  P --> OS
  G[TITAN LOCK Governance] --> OS
  CANON[44-Domain Enterprise Canon] --> G
  REF[Frozen Apps Script v1.0] --> G
  G --> SMP[SMP1 Standalone]
  SMP --> PROD[Standalone Production]
  PROD --> V11[Version 1.1 / ERP9 / ERP10 later]
```

## Working principle
GiftHatkeOS is not merely an Apps Script project. It is a **runtime-independent business platform** covering CRM, customers, orders, production, inventory, finance, shipping, permissions, documentation, workflows, validation/business rules and SOPs. Apps Script is the permanent reference implementation; standalone is the target runtime.
