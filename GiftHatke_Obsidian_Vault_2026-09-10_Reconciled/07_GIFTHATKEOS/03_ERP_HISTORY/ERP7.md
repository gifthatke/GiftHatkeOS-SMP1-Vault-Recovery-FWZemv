---
title: "ERP7"
status: "certified-historical"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatkeos
  - erp7
  - permissions
---

# ERP7

Retained subprograms:
- ERP7.1 User Management Foundation RC1
- ERP7.2 Permission Catalogue RC1
- ERP7.3 Enterprise Role Management
- ERP7.4 User–Role Assignment

Historical issues mentioned during development:
- missing test/runner functions such as `runErp72PermissionTests`;
- role/permission cache or API errors such as `users.roles.create` and `roleInvalidateCache`.

ERP7 established enterprise user/role/permission foundations later relied upon by production RBAC.
