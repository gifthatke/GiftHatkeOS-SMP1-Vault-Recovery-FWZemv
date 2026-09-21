# Recovery controls — source reconciliation

Date: 2026-09-13. Classification: **partial foundation** for full Canon Domain 38 operational assurance. Pack 2.11 and S2-OP-01 retain their certified controlled closure.

## Decision

The earlier working matrix's recovery candidates now have directly inspected source and certificate support. The implementation preserves a bounded backup-metadata and recovery-readiness foundation. It does not demonstrate a physical production backup or tested production-data recovery.

This is an explicit retained operational boundary, not a newly discovered reason to reopen Pack 2.11. Its closure certificate expressly excludes physical backup execution, artifact storage, cryptographic artifact verification, provider topology, RPO, RTO and time-based retention. None of those values or provider decisions may be invented.

## Exact authorities

- Canon: [[Domain-38-Stage-10-Original]], §10.88–95; Domain 39 §10.85–99; Domain 41 §10.115–117.
- Active source: GiftHatkeOS-Standalone at `06090ddcd8a68c6be6ef484d43b252547bf5ec55`.
- Frozen source: GiftHatkeOS at `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`.
- [[Pack-2-11-Ownership-Contract-Original]] and [[Pack-2-11F-Controlled-Closure-Original]] preserve the original repository certificate text.
- [[Recovery-Control-Source-Provenance.json]] records exact source paths, Git blob IDs and SHA-256 hashes for the ten documents inspected.

## Behavior comparison

| Control | Frozen behavior | Standalone evidence | Classification and limit |
|---|---|---|---|
| Backup registration | BackupService.js stores descriptor metadata, requires location and caps the registry at 50 | backup-recovery-service.ts delegates normalized records to a repository; backup-recovery-metadata.ts declares the 50-record limit | complete for the previously certified metadata-registration scope; no physical backup inferred |
| Backup verification | backupVerify_ checks location, version and FAILED status | verifyBackup calls the Domain descriptor verifier and persists its result | complete for the previously certified metadata-validation scope; no artifact checksum recomputation proved |
| Recovery-point registration | RecoveryService.js rejects a supplied unknown backup; registry capped at 30 | registerRecoveryPoint checks supplied backup existence before persistence; database limit is 30 | complete for the previously certified bounded registration scope |
| Recovery readiness | Frozen validation checks release identity and an attached verified backup | validateRecovery delegates to the Domain validator and persists readiness | complete for the previously certified readiness scope; READY does not establish real restore success |
| Read-only observation | Pack 2.11 contract records the frozen ERP8.10 before/after dry-run boundary | restoreDryRun captures before/after fingerprints; Platform fingerprint contains the two metadata registries only | complete for the previously certified metadata observation scope; not a fingerprint of all business tables |
| Artifact integrity | Frozen checksum is metadata | Platform explicitly describes FNV-1a as a comparison token, not backup authenticity verification | partial foundation for operational artifact assurance; no stronger guarantee inferred |
| Runtime transport | Frozen services include summary/validation public wrappers; the standalone Pack 2.11 contract authorizes internal orchestration | composition test explicitly requires internal service composition and no backup/recovery HTTP routes or ApiServices exposure | complete for the certified internal scope; absence of a new UI/route is not automatically a defect |
| Recovery actor/auditability | Frozen registration preserves actor and emits audit events | standalone service and repository preserve actor metadata; Pack 2.11 closure certifies its bounded foundation | partial foundation for any stronger audit-event equivalence claim; no dedicated event-sink equivalence independently established here |

The `complete` classifications above refer only to the precisely stated historical certified subcontrols. No tests were executed in this pass, and none is a new live acceptance certificate.

## Canon minimum operational evidence

Domain 38 §10.95 requires the following evidence before final standalone certification. No current production evidence for these items was recovered from the bounded source set below; that is not proof that the controls do not exist elsewhere.

| Canon requirement | Evidence still required | Classification |
|---|---|---|
| Critical services classified | Actual production critical-service inventory | partial foundation |
| Recovery objectives defined | Approved RPO/RTO values and applicability | partial foundation |
| Dependencies understood | Current Render/Neon/storage/identity recovery dependency record | partial foundation |
| Backup configured | Current provider configuration attestation | partial foundation |
| Backup successfully created | Identified successful backup execution record | partial foundation |
| Restore successfully tested | Isolated restore execution and result evidence | partial foundation |
| Identity/permissions recovered | Restored access-control validation | partial foundation |
| Critical workflows validated | Business workflow checks against the recovered environment | partial foundation |
| Reconciliation demonstrated | Source/restore data reconciliation results | partial foundation |
| Recovery authority documented | Approved operator ownership and recovery decision procedure | partial foundation |
| Dual-writer control validated | Evidence identifying and controlling the authoritative production writer | partial foundation |
| Business recovery acceptance | Recorded business acceptance of the demonstrated recovery | partial foundation |

This governance wave authorizes evidence recovery and review only. It does not authorize a restore drill, provider changes, production writes, a backup UI, new permissions, or selecting operational recovery targets.

## Scope of inspection

Read in full: frozen BackupService.js and RecoveryService.js; standalone Platform backup/recovery adapter; application backup/recovery service; production composition test; Platform backup/recovery test; Pack 2.11 ownership contract; Pack 2.11F controlled closure; earlier standalone go-live closure certificate. Database metadata source was inspected for its declared models, persistence boundary and registry caps, without claiming complete line-by-line persistence verification.

The earlier go-live certificate explicitly states that production deployment and real database migration were not performed by that certification. Its readiness percentage therefore cannot substitute for current production recovery or cutover evidence.

## Finding and programme disposition

- GAP-001 remains blocking: source recovery is complete, but exhaustive 44-domain semantic parity is not yet certified.
- GAP-009 remains blocking: this review supplies direct authority for the retained operational recovery evidence requirement. It does not merge or remove another finding.
- GAP-002, 005, 006 and 011 remain blocking; all thirteen existing findings retain their classifications and remain open.
- Domain 38 remains partial foundation for its full operational assurance boundary. Domains 39 and 41 consume the missing recovery/authority evidence and remain partial foundation.
- The latest live acceptance attempt remains blocked by Google sign-in returning 502 in the cloud browser. No fresh browser check occurred in this source-only pass.
- No commit, push, deployment, test execution, production-data operation or frozen-repository mutation occurred.

The smallest next evidence-authorized step is to locate and reconcile existing production backup/restore and authoritative-writer records under the current governance wave. If those records do not exist, the corresponding operational work requires its own Titan Lock; it must not be implemented during this wave.

Canon Recovery wave: 80% (estimate; final reconciliation and publication outstanding). User Management: 100% read-only closure. Settings: 100% current-wave closure. Overall SMP1: less than 100%; handover blocked.
