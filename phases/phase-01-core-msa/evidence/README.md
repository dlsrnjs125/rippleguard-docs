# Phase 1 Evidence

Phase 1 Evidence에는 구현 Repository별 PR, 병합 commit, 검증 명령, CI Run, Docker Compose E2E 결과와 Baseline 값을 기록한다.

현재 Phase 1 구현 Repository PR은 main에 병합됐지만, image provenance와 Docker Compose runtime verification이 완료되지 않아 Phase 1은 아직 `VERIFIED`가 아니다.

## Evidence

| 영역 | Evidence | 상태 |
| --- | --- | --- |
| Contracts PR | [rippleguard-contracts #3](https://github.com/dlsrnjs125/rippleguard-contracts/pull/3), merge `29f6c348fd93633476438ee36b3f93a3d036e165`, GitHub check `validate` success | CONFIRMED |
| Contracts local validation | `make validate`: 30 schemas, 1 OpenAPI document, 50 valid examples, 78 intentional invalid examples, 8 scenarios, 16 fixture-backed compatibility checks | PASS |
| Loan Service PR | [rippleguard-loan-service #1](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/1), merge `54ea344a682723d61d9beedf4ade56ee48029c0d`, GitHub check `test-package-docker` success | CONFIRMED |
| Loan local tests | `./mvnw test`: 24 tests, 0 failures, 0 errors | PASS |
| Governance Service PR | [rippleguard-governance-service #1](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/1), merge `29bafba34c47e003fdefafa455924992993721cf`, GitHub check `test-package-docker` success | CONFIRMED |
| Governance local tests | `./mvnw test`: 16 tests, 0 failures, 0 errors | PASS |
| Audit Foundation PR | [rippleguard-audit-replay-service #1](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/1), merge `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`, GitHub check `test` success | CONFIRMED |
| Audit local tests | `./mvnw test`: 17 tests, 0 failures, 0 errors | PASS |
| Infra PR | [rippleguard-infra #2](https://github.com/dlsrnjs125/rippleguard-infra/pull/2), merge `ebdd864b279e39701f2bb21750a5080a827943f2`, GitHub check `static-validation` success | CONFIRMED |
| Infra static validation | `make validate-static`: contract baseline, topic baseline, manifest and secret checks passed | PASS |
| Image provenance | `python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` | FAIL: commit-tagged images lack expected OCI labels |
| Runtime E2E | `make phase1-e2e` | NOT_RUN |
| Duplicate Test | `make phase1-duplicate-check` | NOT_RUN |
| Recovery Test | `make phase1-recovery-check`; `make phase1-outbox-recovery-check` | NOT_RUN |
| Timeline Test | `make phase1-order-check` | NOT_RUN |
| Docs finalization PR | `docs/phase-1-baseline-finalization` | IN_REVIEW |

## Image Manifest Snapshot

| Service | Image Tag | Digest | Expected OCI Revision | Migration |
| --- | --- | --- | --- | --- |
| Loan | `rippleguard-loan-service:54ea344a6827` | TBD / `null` in manifest | `54ea344a682723d61d9beedf4ade56ee48029c0d` | `V1__loan_core.sql` |
| Governance | `rippleguard-governance-service:29bafba34c47` | TBD / `null` in manifest | `29bafba34c47e003fdefafa455924992993721cf` | `V1__governance_core.sql` |
| Audit | `rippleguard-audit-replay-service:e7d9d9f8afb1` | TBD / `null` in manifest | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | `V1__audit_foundation.sql` |
