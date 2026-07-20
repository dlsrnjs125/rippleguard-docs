# Phase 1 Evidence

Phase 1 Evidence에는 구현 Repository별 PR, 병합 commit, 검증 명령, CI Run, Docker Compose E2E 결과와 Baseline 값을 기록한다.

현재 Phase 1 구현 Repository PR은 main에 병합됐지만, image provenance와 Docker Compose runtime verification이 완료되지 않아 Phase 1은 아직 `VERIFIED`가 아니다.

## Confirmed Evidence

| Repository | Evidence | Result |
| --- | --- | --- |
| `rippleguard-contracts` | [PR #3](https://github.com/dlsrnjs125/rippleguard-contracts/pull/3), merge `29f6c348fd93633476438ee36b3f93a3d036e165`, GitHub check `validate` | PASS |
| `rippleguard-contracts` | `make validate`: 30 schemas, 1 OpenAPI document, 50 valid examples, 78 intentional invalid examples, 8 scenarios, 16 fixture-backed compatibility checks | PASS |
| `rippleguard-loan-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/1), merge `54ea344a682723d61d9beedf4ade56ee48029c0d`, GitHub check `test-package-docker` | PASS |
| `rippleguard-loan-service` | `./mvnw test`: 24 tests, 0 failures, 0 errors | PASS |
| `rippleguard-governance-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/1), merge `29bafba34c47e003fdefafa455924992993721cf`, GitHub check `test-package-docker` | PASS |
| `rippleguard-governance-service` | `./mvnw test`: 16 tests, 0 failures, 0 errors | PASS |
| `rippleguard-audit-replay-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/1), merge `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`, GitHub check `test` | PASS |
| `rippleguard-audit-replay-service` | `./mvnw test`: 17 tests, 0 failures, 0 errors | PASS |
| `rippleguard-infra` | [PR #2](https://github.com/dlsrnjs125/rippleguard-infra/pull/2), merge `ebdd864b279e39701f2bb21750a5080a827943f2`, GitHub check `static-validation` | PASS |
| `rippleguard-infra` | `make validate-static`: contract baseline, topic baseline, manifest and secret checks | PASS |
| `rippleguard-docs` | `git diff --check`, Markdown link check, contract/topic comparison, manifest/source commit comparison | PASS |

## Missing Evidence

| Verification | Reason Missing | Owner | Required Artifact |
| --- | --- | --- | --- |
| Loan OCI provenance | Required labels absent | `rippleguard-loan-service` | Image inspect summary showing tag, source commit, OCI revision and OCI source |
| Governance OCI provenance | Required labels absent | `rippleguard-governance-service` | Image inspect summary showing tag, source commit, OCI revision and OCI source |
| Audit OCI provenance | Required labels absent | `rippleguard-audit-replay-service` | Image inspect summary showing tag, source commit, OCI revision and OCI source |
| Runtime E2E | Blocked by image provenance | `rippleguard-infra` | Sanitized `phase1-e2e` summary |
| Duplicate test | Runtime stack baseline not reached | `rippleguard-infra` | Sanitized duplicate-check summary |
| Recovery test | Runtime stack baseline not reached | `rippleguard-infra` | Sanitized recovery and outbox-recovery summaries |
| Timeline order test | Runtime stack baseline not reached | `rippleguard-infra` | Sanitized order-check summary |
| Final Phase 1 published baseline | Runtime evidence missing | `rippleguard-docs` | Docs PR updating Phase 1 to `VERIFIED` and Phase 2 to `READY` after Infra PASS |

## Evidence Acceptance Criteria

Image provenance evidence must include:

- Repository.
- Source commit.
- Immutable image tag.
- `org.opencontainers.image.revision`.
- `org.opencontainers.image.source`.
- Verification command.
- PASS result.

Runtime E2E evidence must include:

- Infra commit.
- Service image tags.
- Contract commit.
- Migration versions.
- Command.
- Sanitized result.

Logs should be summarized, not copied wholesale. Evidence must be enough to reproduce the verification decision without exposing raw application payloads or secrets.

## Image Manifest Snapshot

| Service | Image Tag | Digest | Expected OCI Revision | Migration |
| --- | --- | --- | --- | --- |
| Loan | `rippleguard-loan-service:54ea344a6827` | TBD / `null` in manifest | `54ea344a682723d61d9beedf4ade56ee48029c0d` | `V1__loan_core.sql` |
| Governance | `rippleguard-governance-service:29bafba34c47` | TBD / `null` in manifest | `29bafba34c47e003fdefafa455924992993721cf` | `V1__governance_core.sql` |
| Audit | `rippleguard-audit-replay-service:e7d9d9f8afb1` | TBD / `null` in manifest | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | `V1__audit_foundation.sql` |
