---
title: "Standalone Production Architecture"
status: "live-observed"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatkeos
  - standalone
  - architecture
---

# Standalone Production Architecture

Latest retained production foundation:
- hosting/runtime: **Render**
- database: **Neon PostgreSQL**
- authentication: **Google**
- session: secure **HttpOnly** cookie/session
- authorization: production **RBAC**
- privileged role: **SUPER_ADMIN**
- custom domain: **erp.gifthatke.in**
- **TLS**
- employee workspaces observed live for Orders, Production and Inventory.

## Design objective
Replace Apps Script runtime limitations without changing the certified business model.
