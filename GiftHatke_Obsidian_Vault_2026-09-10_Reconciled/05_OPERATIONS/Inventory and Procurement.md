---
title: "Inventory and Procurement"
status: "knowledge-snapshot"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatke
  - inventory
  - procurement
  - ux
---

# Inventory and Procurement

## UX decisions retained
### Purchase suggestion
A historical issue showed stock as 0 and required diagnosis/deployment work.

### Category / Supplier inputs
- should remain blank initially with example/help;
- support dropdown/autocomplete from saved values;
- user should select existing values;
- avoid duplicate master-data creation.

### Stock settings
- **Current Stock** — read-only ledger-derived value.
- **Opening Stock** — allowed on new material; creates a stock movement.
- **Stock On Order** — read-only, derived from procurement.

## Standalone state
Inventory workspace was observed live/200 in the production foundation snapshot.
