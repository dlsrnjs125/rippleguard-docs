# Phase 2 Verification

Final cross-repository verification was last reconciled on 2026-07-24. Phase 2 remains `BLOCKED`; see [Final Cross-Repository Verification](final-review.md).

## Verification Matrix

| 검증 항목 | Owner | 명령 | Expected | Actual | Result | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| Contract Validation | `rippleguard-contracts`, `rippleguard-infra` | `make validate-static` | PASS | PASS | PASS | `../rippleguard-infra/evidence/phase2/final-verification.json` |
| Loan Snapshot Persistence | Loan, Infra | `make phase2-e2e` | Immutable snapshot ID/version/digest present | Present | PASS | `happy-path.json` |
| Snapshot Timestamp Identity | Loan, Governance, Infra | `make phase2-e2e` | DB `created_at` = API `createdAt` = reference `snapshotCreatedAt` | Equal | PASS | `happy-path.json` |
| Governance Snapshot Acquisition | Governance, Loan | `make phase2-e2e` | Evaluation Run stores Loan API snapshot identity and feature digest | Match | PASS | `happy-path.json` |
| Agent Runtime Determinism | Agent Runtime, Infra | `make phase2-reproducibility-check` | Artifact digests match manifest | Match | PASS | `reproducibility.json` |
| Request Event Causation | Governance, Audit | `make phase2-e2e` | `validation.causationId == request.eventId` and not `agentRunId` | Match | PASS | `happy-path.json` |
| Audit Pending Reconciliation | Audit | `make phase2-e2e` | No stale pending/quarantine row for validation event | Pending absent, quarantine `0` | PASS | `happy-path.json` |
| Image Provenance | Infra, all services | `make phase2-verify-images` | commit-tagged image, local image ID, OCI source/revision | Match | PASS_LOCAL_BASELINE | `phase2-loan-decision.json` |
| Happy Path E2E | Infra, all services | `make phase2-e2e` | Loan -> Governance -> Agent Runtime -> Audit succeeds | `PROPOSAL_READY` | PASS | `happy-path.json` |
| Retry | Infra/Governance/Runtime | `make phase2-retry-check` | PASS | exit `2` | BLOCKED | `retry.json` |
| Timeout | Infra/Governance/Runtime | `make phase2-timeout-check` | PASS | exit `2` | BLOCKED | `timeout.json` |
| Duplicate Request | Infra/Governance/Runtime | `make phase2-duplicate-request-check` | PASS | exit `2` | BLOCKED | `duplicate-request.json` |
| Duplicate Result | Infra/Governance/Audit | `make phase2-duplicate-result-check` | PASS | exit `2` | BLOCKED | `duplicate-result.json` |
| Conflict | Infra/Governance/Audit | `make phase2-conflict-check` | PASS | exit `2` | BLOCKED | `conflict.json` |
| Artifact Digest Failure | Infra/Agent Runtime | `make phase2-artifact-digest-failure-check` | PASS | exit `2` | BLOCKED | `artifact-failure.json` |
| Missing Artifact | Infra/Agent Runtime | `make phase2-missing-artifact-check` | PASS | exit `2` | BLOCKED | `missing-artifact.json` |
| Contract Mismatch | Infra/Contracts/services | `make phase2-contract-mismatch-check` | PASS | exit `2` | BLOCKED | `contract-mismatch.json` |
| Snapshot Mismatch | Infra/Loan/Governance | `make phase2-snapshot-mismatch-check` | PASS | exit `2` | BLOCKED | `snapshot-mismatch.json` |
| Recovery | Infra/all services | `make phase2-recovery-check` | PASS | exit `2` | BLOCKED | `recovery.json` |
| Reproducibility | Infra/Agent Runtime | `make phase2-reproducibility-check` | PASS | PASS | PASS | `reproducibility.json` |
| Local LLM Absence | Infra/Agent Runtime | `make phase2-local-llm-absent-check` | PASS | PASS | PASS | `local-llm-absent.json` |

## Status Rule

Phase 2 must remain `BLOCKED` until all required failure drills pass with real runtime injection, `make phase2-verify` exits `0`, the infra manifest records `publicationStatus = PUBLISHED`, `verification.status = PASSED`, and `knownBlockers = []`.

`PASS_LOCAL_BASELINE` means a local Docker Compose image ID was verified. It is not a production registry digest.
