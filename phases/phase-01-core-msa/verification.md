# Phase 1 Verification

## 검증 목표

AI 없는 Core MSA 최소 수직 흐름이 계약, 상태 소유권, 멱등성, 독립 DB와 Audit Foundation 기준을 만족하는지 검증한다.

## 환경

| 항목 | 값 |
| --- | --- |
| 실행 시각 | 2026-07-20 09:30 KST |
| 관련 Commit | Contracts `29f6c348fd93633476438ee36b3f93a3d036e165`; Loan `54ea344a682723d61d9beedf4ade56ee48029c0d`; Governance `29bafba34c47e003fdefafa455924992993721cf`; Audit `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`; Infra `ebdd864b279e39701f2bb21750a5080a827943f2` |
| 환경·도구 버전 | macOS local shell, Java 17.0.19, Docker Desktop 29.5.3, GitHub Checks API |

## 검증 항목

| 항목 | 명령·방법 | 기대 결과 | 실행 결과 | 상태 |
| --- | --- | --- | --- | --- |
| Loan 신청 REST 동작 | `rippleguard-loan-service ./mvnw test` | 신청 요청이 검증되고 신청 ID가 반환됨 | Service integration tests PASS; Compose REST E2E NOT_RUN | PARTIAL |
| Versioned Snapshot 저장 | `rippleguard-loan-service ./mvnw test` | 신청 입력과 Snapshot이 versioned record로 저장됨 | PASS, 24 tests total | PASS |
| Submitted Event 발행 | `rippleguard-loan-service ./mvnw test` | `loan.application.submitted.v1`이 outbox를 통해 발행됨 | PASS | PASS |
| Decision Case 생성 | `rippleguard-governance-service ./mvnw test` | Governance Service가 신청 Event로 Decision Case를 1회 생성함 | PASS, 16 tests total | PASS |
| Evaluation Run | `rippleguard-governance-service ./mvnw test` | Decision Case 생성과 함께 Evaluation Run이 저장됨 | PASS | PASS |
| Mock Evaluation 재현 | `rippleguard-governance-service ./mvnw test` | 고정 Mock Evaluator가 동일 입력에 동일 Decision Envelope를 생성함 | PASS | PASS |
| Decision Command 적용 | `rippleguard-loan-service ./mvnw test`; `rippleguard-governance-service ./mvnw test` | Governance Command를 Loan Service가 검증 후 적용함 | PASS | PASS |
| 최종 상태 1회 반영 | `rippleguard-loan-service ./mvnw test` | Loan 최종 상태가 한 번만 전이됨 | PASS | PASS |
| 중복 Event 멱등 처리 | service tests | 중복 Event가 중복 Case 또는 중복 최종 결정을 만들지 않음 | PASS in service tests; Infra duplicate script NOT_RUN | PARTIAL |
| Consumer 재시작 | `rippleguard-infra make phase1-recovery-check` | Consumer 재시작 후 미처리 Event가 복구됨 | NOT_RUN | PENDING |
| Timeline | `rippleguard-audit-replay-service ./mvnw test`; `rippleguard-infra make phase1-order-check` | 신청부터 최종 상태까지 최소 Timeline 조회 가능 | Audit service tests PASS; Infra order/timeline script NOT_RUN | PARTIAL |
| 독립 DB 소유권 | service migrations; Infra compose config | Loan/Governance/Audit DB 직접 교차 접근 없음 | Static config and migration ownership confirmed | PASS |
| Docker Compose | `rippleguard-infra make validate-static`; Phase 1 runtime scripts | 빈 환경에서 최소 수직 흐름 재현 | Static validation PASS; runtime E2E NOT_RUN | PARTIAL |
| Contract 호환성 | `rippleguard-contracts make validate`; `rippleguard-infra make validate-static` | Contract baseline, schema, examples, fixtures and topics match | PASS | PASS |
| Image와 Commit 대조 | `rippleguard-infra python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` | commit-tagged images expose expected OCI source/revision labels | FAIL: local images lack expected labels | FAIL |
| CI | GitHub Checks API on main commits | 각 main merge commit의 CI check success | PASS for Contracts, Loan, Governance, Audit, Infra | PASS |

## 실패 항목

- `rippleguard-infra` image verification failed: local commit-tagged images do not expose `org.opencontainers.image.revision` or `org.opencontainers.image.source`.
- Docker Compose runtime checks were not marked PASS: `phase1-e2e`, `phase1-duplicate-check`, `phase1-recovery-check`, `phase1-outbox-recovery-check` and `phase1-order-check` were not completed after image provenance failed.
- Initial service test attempts inside the sandbox failed because Docker/Testcontainers socket access and Mockito Byte Buddy agent self-attach were blocked; the same service tests passed after approved execution outside the sandbox.

## 잔여 위험

- `imageDigest` remains `null` in the Infra manifest; commit tags and OCI labels are the current provenance mechanism, but the local images did not satisfy that label check.
- Flyway checksum baselines remain `null`; version, description and script are recorded.
- Phase 1 cannot be declared `VERIFIED` until image provenance and Docker Compose runtime checks pass.
