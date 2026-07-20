# Phase 1 Cross-Repo Baseline

Phase 1 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 Published Baseline으로 사용하지 않는다.

## Repository Order

1. `rippleguard-contracts`
2. `rippleguard-loan-service`
3. `rippleguard-governance-service`
4. `rippleguard-audit-replay-service`
5. `rippleguard-infra`
6. `rippleguard-docs` finalization

현재 Repository PR이 main에 병합된 뒤 다음 Repository 작업을 시작한다. 한 번에 하나의 Repository만 수정한다. 각 서비스 상세 구현은 해당 Repository에서 관리하고, 중앙 Docs에는 Cross-Repo 진행 상태와 검증 Baseline만 기록한다.

| Repository | Branch | PR | Candidate Baseline | Published Baseline | Docker Image | Contract Version | DB Migration | Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-contracts` | main | [#3](https://github.com/dlsrnjs125/rippleguard-contracts/pull/3) | `29f6c348fd93633476438ee36b3f93a3d036e165` | `29f6c348fd93633476438ee36b3f93a3d036e165` | N/A | Phase 1 OpenAPI `1.0.0`, Event schemas `v1.0.0`/`v1.1.0` | N/A | GitHub check `validate` success; local `make validate` PASS | VERIFIED_FOR_CONTRACTS |
| `rippleguard-loan-service` | main | [#1](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/1) | `54ea344a682723d61d9beedf4ade56ee48029c0d` | `54ea344a682723d61d9beedf4ade56ee48029c0d` | `rippleguard-loan-service:54ea344a6827`; digest TBD | Contract commit `29f6c348fd93633476438ee36b3f93a3d036e165` | `V1__loan_core.sql` | GitHub check `test-package-docker` success; local `./mvnw test` PASS | IMPLEMENTED_PENDING_IMAGE_E2E |
| `rippleguard-governance-service` | main | [#1](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/1) | `29bafba34c47e003fdefafa455924992993721cf` | `29bafba34c47e003fdefafa455924992993721cf` | `rippleguard-governance-service:29bafba34c47`; digest TBD | Contract commit `29f6c348fd93633476438ee36b3f93a3d036e165` | `V1__governance_core.sql` | GitHub check `test-package-docker` success; local `./mvnw test` PASS | IMPLEMENTED_PENDING_IMAGE_E2E |
| `rippleguard-audit-replay-service` | main | [#1](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/1) | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | `rippleguard-audit-replay-service:e7d9d9f8afb1`; digest TBD | Contract commit `29f6c348fd93633476438ee36b3f93a3d036e165` | `V1__audit_foundation.sql` | GitHub check `test` success; local `./mvnw test` PASS | IMPLEMENTED_PENDING_IMAGE_E2E |
| `rippleguard-infra` | main | [#2](https://github.com/dlsrnjs125/rippleguard-infra/pull/2) | `ebdd864b279e39701f2bb21750a5080a827943f2` | `ebdd864b279e39701f2bb21750a5080a827943f2` | Manifest commit tags above; `imageDigest: null` | Baseline commit `29f6c348fd93633476438ee36b3f93a3d036e165` | Manifest records service V1 migrations | GitHub check `static-validation` success; local `make validate-static` PASS; local `verify-phase1-images.py` FAIL | BLOCKED_ON_IMAGE_PROVENANCE |
| `rippleguard-docs` | `docs/phase-1-baseline-finalization` | TBD | branch HEAD | TBD | N/A | N/A | N/A | `git diff --check` PASS; Markdown link check PASS | IN_REVIEW |

PR 중에는 Candidate Baseline을 `PR Head`로 표시하고 `IN_REVIEW`로 관리한다. 병합 뒤 별도 metadata update에서 Merge Commit SHA 또는 승인된 Phase Tag를 Published Baseline으로 고정한 뒤에만 `VERIFIED`로 올린다.

## Baseline Decision

Phase 1은 아직 `VERIFIED`가 아니다. GitHub PR, merge commit, CI, contract validation and service tests are confirmed, but local image provenance verification failed because the commit-tagged service images do not expose the expected OCI labels. Docker Compose E2E, duplicate, recovery, outbox recovery and timeline checks were therefore not recorded as PASS.
