# Phase 1 Progress

| 작업 | 상태 | 비고 |
| --- | --- | --- |
| Phase 1 Kickoff | MERGED | `rippleguard-docs` PR #6 merged at `d18231e55d891eb2f73736381cc3921289b73d2b` |
| Contracts | MERGED | `rippleguard-contracts` PR #3 merged at `29f6c348fd93633476438ee36b3f93a3d036e165`; `make validate` PASS |
| Loan Service | MERGED | `rippleguard-loan-service` PR #1 merged at `54ea344a682723d61d9beedf4ade56ee48029c0d`; `./mvnw test` PASS with Docker/Testcontainers access |
| Governance Service | MERGED | `rippleguard-governance-service` PR #1 merged at `29bafba34c47e003fdefafa455924992993721cf`; `./mvnw test` PASS with Docker/Testcontainers access |
| Audit Foundation | MERGED | `rippleguard-audit-replay-service` PR #1 merged at `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`; `./mvnw test` PASS with Docker/Testcontainers access |
| Infra Integration | IN_REVIEW | `rippleguard-infra` PR #2 merged at `ebdd864b279e39701f2bb21750a5080a827943f2`; `make validate-static` PASS, but local image OCI label verification failed |
| E2E | BLOCKED | Docker Compose E2E, duplicate, recovery, outbox recovery and order/timeline scripts were not run to PASS because image provenance failed first |
| Baseline | IN_REVIEW | main commits, PRs, CI and contract/migration metadata are recorded; image provenance and local E2E evidence remain incomplete |

Phase 1을 `VERIFIED`로 전환하려면 image provenance 검증과 Docker Compose 통합 검증이 모두 PASS여야 한다. 현재 상태에서는 Phase 2를 `READY`로 전환하지 않는다.

## Phase 1 Contract Scope

Phase 1의 `rippleguard-contracts` 작업은 Phase 0 Published Contract를 다시 설계하거나 덮어쓰는 작업이 아니다.

신규·보완 범위는 다음으로 제한한다.

- Loan Application REST/OpenAPI 신규 계약
- Versioned Snapshot 입력·조회 계약
- 기존 Phase 0 Event의 서비스 적용 Fixture와 DTO 경계
- Outbox·Inbox 처리에 필요한 Header·멱등성 사용 규칙
- Mock Evaluation 서비스 적용 Fixture
- Consumer compatibility 검증

Phase 0 Published Contract는 직접 수정하지 않는다. 호환 가능한 변경은 새 minor schema로 추가하고, 비호환 변경은 새 major generation과 ADR을 요구한다.
