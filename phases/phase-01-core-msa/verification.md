# Phase 1 Verification

## 검증 목표

AI 없는 Core MSA 최소 수직 흐름이 계약, 상태 소유권, 멱등성, 독립 DB와 Audit Foundation 기준을 만족하는지 검증한다.

## 환경

| 항목 | 값 |
| --- | --- |
| 실행 시각 | 2026-07-20 09:30 KST |
| 관련 Commit | Contracts `29f6c348fd93633476438ee36b3f93a3d036e165`; Loan `54ea344a682723d61d9beedf4ade56ee48029c0d`; Governance `29bafba34c47e003fdefafa455924992993721cf`; Audit `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`; Infra `ebdd864b279e39701f2bb21750a5080a827943f2` |
| 환경·도구 버전 | macOS local shell, Java 17.0.19, Docker Desktop 29.5.3, GitHub Checks API |

## Verification Matrix

| 항목 | 명령·방법 | 결과 | 상태 | 실패 원인 | 책임 Repository | 재검증 조건 |
| --- | --- | --- | --- | --- | --- | --- |
| Contract validation | `rippleguard-contracts make validate` | 30 schemas, 1 OpenAPI, examples, scenarios and compatibility checks passed | PASS | N/A | `rippleguard-contracts` | N/A |
| Loan service tests | `rippleguard-loan-service ./mvnw test` | 24 tests passed | PASS | N/A | `rippleguard-loan-service` | N/A |
| Governance service tests | `rippleguard-governance-service ./mvnw test` | 16 tests passed | PASS | N/A | `rippleguard-governance-service` | N/A |
| Audit service tests | `rippleguard-audit-replay-service ./mvnw test` | 17 tests passed | PASS | N/A | `rippleguard-audit-replay-service` | N/A |
| Infra static validation | `rippleguard-infra make validate-static` | Compose config, topic baseline, manifest and secret checks passed | PASS | N/A | `rippleguard-infra` | N/A |
| CI | GitHub Checks API on main commits | Contracts, Loan, Governance, Audit and Infra checks succeeded | PASS | N/A | Each repository | N/A |
| Loan image provenance | `rippleguard-infra python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` | Expected OCI labels missing | FAIL | Docker image lacks `org.opencontainers.image.revision` and `org.opencontainers.image.source` | `rippleguard-loan-service` | Docker build PR merged, image rebuilt, image verification PASS |
| Governance image provenance | same command | Expected OCI labels missing | FAIL | Docker image lacks `org.opencontainers.image.revision` and `org.opencontainers.image.source` | `rippleguard-governance-service` | Docker build PR merged, image rebuilt, image verification PASS |
| Audit image provenance | same command | Expected OCI labels missing | FAIL | Docker image lacks `org.opencontainers.image.revision` and `org.opencontainers.image.source` | `rippleguard-audit-replay-service` | Docker build PR merged, image rebuilt, image verification PASS |
| Phase 1 compose check | `rippleguard-infra make phase1-check` | Not executed to PASS | BLOCKED_BY_IMAGE_PROVENANCE | Immutable service images are not provenance-valid | `rippleguard-infra`; blocked by service image PRs | All three image provenance checks PASS |
| Runtime E2E | `rippleguard-infra make phase1-e2e` | Not executed to PASS | BLOCKED_BY_IMAGE_PROVENANCE | Stack verification must not proceed from unverified images | `rippleguard-infra` | `phase1-check` PASS |
| Duplicate test | `rippleguard-infra make phase1-duplicate-check` | Not executed to PASS | BLOCKED_BY_IMAGE_PROVENANCE | Runtime stack verification not established | `rippleguard-infra` | Runtime E2E baseline established |
| Recovery test | `rippleguard-infra make phase1-recovery-check`; `make phase1-outbox-recovery-check` | Not executed to PASS | BLOCKED_BY_IMAGE_PROVENANCE | Runtime stack verification not established | `rippleguard-infra` | Runtime E2E baseline established |
| Timeline order test | `rippleguard-infra make phase1-order-check` | Not executed to PASS | BLOCKED_BY_IMAGE_PROVENANCE | Audit runtime image is not provenance-valid | `rippleguard-infra` | Audit image validated and `phase1-check` PASS |
| Docs checks | `git diff --check`; Markdown link check | Passed | PASS | N/A | `rippleguard-docs` | N/A |

## Service-Level Coverage Confirmed

- Loan REST behavior, Snapshot persistence, submitted outbox event and finalization are covered by Loan service tests.
- Governance Decision Case creation, Evaluation Run persistence, deterministic Mock Evaluation and Decision Command are covered by Governance service tests.
- Audit ingestion, deduplication, privacy sanitization and Timeline behavior are covered by Audit service tests.
- These service-level PASS results do not replace Docker Compose runtime verification.

## Failure Ownership

Root Cause:

- Loan, Governance and Audit service images were built without required OCI labels.

Owner Repositories:

- `rippleguard-loan-service`
- `rippleguard-governance-service`
- `rippleguard-audit-replay-service`

Required Follow-up:

- Add OCI source/revision labels to Docker build.
- Merge service follow-up PRs.
- Rebuild immutable images from recorded main commits.
- Rerun Infra image verification.

Downstream Blocked:

- `rippleguard-infra` runtime verification.
- `rippleguard-docs` Phase 1 `VERIFIED` transition.
- Phase 2 `READY` transition.

## Non-Solution

Do not:

- Weaken Infra verification.
- Manually relabel an unverified image.
- Change only the Infra manifest.
- Mark runtime E2E PASS without image provenance.

## 잔여 위험

- `imageDigest` remains `null` in the Infra manifest; commit tags and OCI labels are the current provenance mechanism.
- Flyway checksum baselines remain `null`; version, description and script are recorded.
- Phase 1 cannot be declared `VERIFIED` until image provenance and Docker Compose runtime checks pass.
