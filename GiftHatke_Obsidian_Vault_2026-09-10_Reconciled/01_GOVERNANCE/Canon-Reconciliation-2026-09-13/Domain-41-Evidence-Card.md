# Domain 41 — Evidence Card

ENTERPRISE PLATFORM ARCHITECTURE, CLOUD OPERATIONS & INFRASTRUCTURE GOVERNANCE CANON

Provisional classification: **partial foundation**. Status: working evidence inventory; not a parity certificate.

## Canon requirement

[[Domain-41-Stage-10-Original]]; original certification state: **Certification Status:** ✅ FULLY CERTIFIED. Message `33a15f70-bec8-42c3-84ba-e8755c9f35a3`; SHA-256 `789c212bcf7419ad3d3556b09763682a832728cb1bb9f128e8b012c67cec9010`. Stage 10 §10.115–117 requires minimum operational infrastructure, secure secrets, tested restore and controlled authority transition. Kubernetes/microservices/multi-cloud are not mandated.

## Frozen Apps Script behavior

[`BackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/BackupService.js) (`481e0f6f243d53a160f0c424b572a41bc63c34b3`); [`ERP73RoleMigration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP73RoleMigration.js) (`99d54c6b818da4ca7ed45e3fcc8bada046f40a51`); [`ERP74AssignmentMigration.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP74AssignmentMigration.js) (`60c44359f5eff6ca0b196fba06ad218484ae06cb`); [`ERP810BackupRecoveryService.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP810BackupRecoveryService.gs) (`d051a3b0f787c23000a1c69f94f3d60f70760289`); [`ERP810PerformanceRecoverySnapshot.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP810PerformanceRecoverySnapshot.gs) (`219dfb7e794349e6f122b7862b06ea1c17885f1c`); [`ERP81SettingsBackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP81SettingsBackupService.js) (`199ea64e074d8c23e210710ecf8724cca672d17f`); [`ERP82CompanyBackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP82CompanyBackupService.js) (`6e3ba53c2dbc1c817baccc1e8d42b742f117f7a1`); [`ERP83OrganizationBackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP83OrganizationBackupService.js) (`60c3edffcee57d64b43c48405d8c07f2151f74c2`); [`ERP84SequenceBackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP84SequenceBackupService.js) (`3ce09a0b29f459165f51b6f612600482939f6362`); [`ERP85TaxFinanceBackupService.js`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP85TaxFinanceBackupService.js) (`1cb71be6c03820b71fbba58490240000ebe43883`); [`ERP87ProductionBackupService.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP87ProductionBackupService.gs) (`5d39b481450e023b0f3d1018495cb92cd8a3bb43`); [`ERP87ProductionMigration.gs`](https://github.com/gifthatke/GiftHatkeOS/blob/fd7c754fb1be380e6d3f9b01dd041b97b82f1d87/ERP87ProductionMigration.gs) (`0cfa4677e1b49ce09d320aa5b56eb882730ccee4`)

These are candidate reference paths; their complete behavior has not been reconciled against every Canon requirement.

## Repository domain implementation

[`packages/domain/src/backup-recovery.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/domain/src/backup-recovery.ts) (`595ac61c8c6a8db0646aa2086c8db584529291c7`)

## Database and persistence

[`packages/database/src/backup-recovery-metadata.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/backup-recovery-metadata.ts) (`c48cc67616dabf968668042f593ecdddf91dea42`); [`packages/database/src/health.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/health.ts) (`a1ac72f8e02104b04be4747bba4c925578a065c3`); [`packages/database/src/migration.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/src/migration.ts) (`f08efe7f19af09b6667896ff5614abc6b9823028`)

Declared matching table candidates: backup_recovery_backups, backup_recovery_points, auth_sessions. Schema declarations do not attest deployed data or migration success.

## Ports

No matching path in this bounded inventory; semantic absence is not established.

Shared application ports are also indexed at apps/api/src/ports.ts; exact method equivalence remains pending.

## Platform adapters

[`packages/platform/src/backup-recovery.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/backup-recovery.ts) (`10dabfd5837ad3536ec3cfad99186a95c14c05bb`); [`packages/platform/src/platform-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/platform-runtime.ts) (`d58480ed650dfc0b228d01f440f0f6b4690f614a`); [`packages/platform/src/security-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/security-runtime.ts) (`8fe2fc8be292338d32f43c230555d81805806779`); [`packages/platform/src/session.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/platform/src/session.ts) (`24fba03632e80060eb6a29a7ecfd31c1d88604d0`)

## Services and routes

[`apps/api/src/authentication/fastify-session-store.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/fastify-session-store.ts) (`1fa98c8efec2ccfd956a475e30a55f667da1b70c`); [`apps/api/src/authentication/session-config.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-config.ts) (`2d4dbb65e2ddd619a469a5d3be19f588c5b5cc34`); [`apps/api/src/authentication/session-runtime.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/authentication/session-runtime.ts) (`6699da66ab00cceed09971692a15a3c7d18bbf3a`); [`apps/api/src/backup-recovery-service.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/backup-recovery-service.ts) (`3612ccd5a10aeba52dedf358cb65868b6bab25b3`); [`apps/api/src/routes/auth-session.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/auth-session.ts) (`e6742641b516b47117ecb924b668a2d0df665893`); [`apps/api/src/routes/health.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/routes/health.ts) (`93a69e5f37054d01156fbbb93ea3f69c6a3ac59f`); [`apps/api/src/runtime-lifecycle.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/runtime-lifecycle.ts) (`467633fb2534e4cc0fcaf51f240209f39e0ac822`); [`apps/api/src/server.ts`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/src/server.ts) (`be5f2522d8a9b1499060167856004d92aef09de5`)

app.ts was read and registers the existing conditional service routes. Registration does not prove production composition or full Canon equivalence.

## Permissions and authentication

The existing overall analysis records Google sessions, CSRF, deny-by-default RBAC and 109 permission keys. This pass does not independently certify all permission-to-operation mappings. Fresh authenticated acceptance is blocked.

## Workspace and UI

No matching path in this bounded inventory; semantic absence is not established.

All closed module-wave decisions remain intact. No new workspace is inferred from this Canon domain.

## Deployment topology

render.yaml declares Render API/static services, external Neon PostgreSQL and autoDeployTrigger: off for both services. This is source configuration, not fresh Render control-plane confirmation. GAP-009 remains open.

## Authenticated live ERP acceptance

2026-09-13 cloud check: ERP sign-in screen reachable; Google sign-in opened a 502 Bad Gateway page. No authenticated acceptance can be certified. Captured ERP-tab error was extension-origin only. The earlier 12/12 acceptance is historical evidence.

## Tests

[`apps/api/test/auth-session-routes.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/auth-session-routes.test.mjs) (`af151f36f11e734add5d4505674c24eef6e97c8c`); [`apps/api/test/backup-recovery-application-service.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/backup-recovery-application-service.test.mjs) (`50ddeb36db44e4c93483099ea9d0f1aa42f756a7`); [`apps/api/test/csrf-runtime.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/csrf-runtime.test.mjs) (`07e239078a416709224f903fb6c04b4ffeb5d167`); [`apps/api/test/fastify-session-store.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/fastify-session-store.test.mjs) (`5e4672aee3836754e1a166bfa32562d864b61670`); [`apps/api/test/production-backup-recovery-composition.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/production-backup-recovery-composition.test.mjs) (`a954ae55030340538ed1dde0a3abbc55fcfc037f`); [`apps/api/test/render-trust-proxy.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/render-trust-proxy.test.mjs) (`9755689510ad9c0c4bea13f3eac39f732ba00af1`); [`apps/api/test/runtime-composition-boundary.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/runtime-composition-boundary.test.mjs) (`13885a4740d6dfe1511549f3271e6c55b1619bce`); [`apps/api/test/runtime-lifecycle.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/runtime-lifecycle.test.mjs) (`9e6ce7beacd98d06415752dd360457d5069dc954`); [`apps/api/test/session-runtime.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/apps/api/test/session-runtime.test.mjs) (`3d67ce33d92746488315f66e3aa0afcb2a902159`); [`packages/database/test/backup-recovery-metadata-persistence.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/backup-recovery-metadata-persistence.test.mjs) (`a88e5f61ce3d332e9b9b3fb4c26f143fefdf1ea3`); [`packages/database/test/migration-foundation.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/migration-foundation.test.mjs) (`cdd087f73e745c92984681a39fe44804d58d882b`); [`packages/database/test/session-persistence.test.mjs`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/packages/database/test/session-persistence.test.mjs) (`e49954d49d7bb97f4320dd449f2031f7453a7d05`)

No test execution in this evidence-only pass. Historical aggregate 1,251/1,252 and complete web 102/103 remain the recorded results (GAP-005).

## Obsidian and governance evidence

[`docs/governance/smp1-canon-recovery-and-final-reconciliation.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-canon-recovery-and-final-reconciliation.md) (`a86edbe9eefba77abc792c1410faac64e0930a00`); [`docs/governance/smp1-canon-recovery-publication-record.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-canon-recovery-publication-record.md) (`a47abb798354f14974968eaf5a5585e8c6454bfa`); [`docs/governance/smp1-procurement-po-runtime-migration-slice-3a4-certification.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-procurement-po-runtime-migration-slice-3a4-certification.md) (`b422b2d688f96c9625a3c0747c7b2ec759f54c4d`); [`docs/governance/smp1-settings-migration-necessity.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-settings-migration-necessity.md) (`2f6c8d7ecd1bc598d518e8d780f257afb057df80`); [`docs/governance/smp1-shipping-s3a7-d-server-composition-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-d-server-composition-authorization.md) (`de3cd1d3219f84c47892d7b29dc42af74d5c199a`); [`docs/governance/smp1-shipping-s3a7-server-composition-authorization.md`](https://github.com/gifthatke/GiftHatkeOS-Standalone/blob/06090ddcd8a68c6be6ef484d43b252547bf5ec55/docs/governance/smp1-shipping-s3a7-server-composition-authorization.md) (`659686c1c77d566bed2e51360b8a0d22d3f4c317`)

See [[SMP1-Finding-Register-Recovery-Addendum-2026-09-13]] and [[Canon-Reconciliation-Checkpoint-2026-09-13]].

## Original requirement excerpt

The following section is reproduced from the linked original, without alteration. It is source evidence, not new implementation authority.

# 10.1 Purpose

Stage 10 consolidates and formally certifies Domain 41 as the permanent GiftHatkeOS platform architecture for standalone runtime, environments, deployment, database operations, configuration, secrets, jobs, observability integration, recovery support, infrastructure governance, and operational acceptance.

The objective is:

> **Ensure GiftHatkeOS has a production-grade standalone platform foundation that is reliable, traceable, recoverable, secure, and scalable enough for real business operations without allowing infrastructure complexity to overtake the business roadmap.**

---
