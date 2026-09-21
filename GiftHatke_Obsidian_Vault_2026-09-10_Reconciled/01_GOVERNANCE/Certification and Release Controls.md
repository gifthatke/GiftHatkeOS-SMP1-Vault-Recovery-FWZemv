---
title: "Certification and Release Controls"
status: "locked-governance"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatke
  - release
  - certification
---

# Certification and Release Controls

## Locked release disciplines retained from ERP/GLP/SMP work
- repository-first evidence;
- immutable references/tags where applicable;
- explicit branch/HEAD verification;
- clean worktree/index gates;
- non-mutation verification for frozen references;
- checksum evidence;
- separate implementation vs certification commits;
- runtime smoke/reconciliation where required;
- regression/API tests registered and executed;
- rollback and verification documentation.

## Historical packaging policy
For the Apps Script engineering baseline, release instructions were locked around:
- single top-level ZIP;
- checksum;
- Verified Executable Functions table;
- PART 1 terminal;
- PART 2 Apps Script wrapper;
- PART 3 Git;
- evidence-based gate.

## Certification language
Avoid “100%” or “closed” unless the specific scope has an explicit certification gate/evidence.
