# Phase 1 — Core MSA Foundation

- Phase: 1
- 상태: `IN_REVIEW`
- 선행 Phase: Phase 0
- 후속 Phase: Phase 2

## 목표와 필요성

AI 없이 대출 신청부터 Governance 처리와 최종 상태 반영까지 최소 수직 흐름을 구현한다. 이후 Agent를 교체 가능한 경계 위에 올리기 전에 Event, 상태 소유권, 멱등성과 독립 DB를 검증한다.

2026-07-20 기준 구현 PR, service-level tests, contract validation and CI checks are confirmed. Phase 1 remains `IN_REVIEW` because service image provenance verification failed locally and Docker Compose runtime E2E checks are not recorded as PASS.

## 범위와 경계

- 포함: 대출 신청·금융 Snapshot, Decision Case, Kafka 신청 Event, Mock Agent·Assurance, Decision Command, Outbox·Inbox, 서비스별 PostgreSQL, 최소 Audit Foundation
- 제외: 실제 모델·LLM Agent, OPA 상세 정책, 문서 RAG, 운영 Console
- 변경 금지: Loan Service만 최종 Loan 상태를, Governance Service만 Decision Case를 변경하며 DB 직접 접근을 금지한다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | 신청·상태·Command 계약 |
| `rippleguard-loan-service` | 신청, Snapshot, Loan 상태 |
| `rippleguard-governance-service` | Decision Case와 Mock 처리 |
| `rippleguard-audit-replay-service` | 원본 Event 저장과 최소 Case Timeline |
| `rippleguard-infra` | Kafka·서비스별 PostgreSQL·Docker 통합 |

## 선행 조건과 산출물

Phase 0의 상태·Event 경계와 초기 계약이 리뷰 가능해야 한다. 산출물은 REST API, Kafka Event, Migration, Docker Image, 최소 E2E Verification과 `correlationId`·`causationId` 기반 원본 Event 저장이다.

Phase 0 handoff에는 Loan Application 상태와 `loan.application.submitted.v1` 제출 Event 계약이 포함된다. 신청 REST/OpenAPI는 Phase 1 구현 산출물이며 Phase 1 진입 조건은 아니다.

## Repository 작업 순서

Phase 1은 다음 순서로 하나의 Repository씩 진행한다.

1. `rippleguard-contracts`
2. `rippleguard-loan-service`
3. `rippleguard-governance-service`
4. `rippleguard-audit-replay-service`
5. `rippleguard-infra`
6. `rippleguard-docs` finalization

Repository 구현 PR은 main에 병합됐다. 중앙 Docs finalization에서는 merge commit, contract, migration, CI and local verification evidence를 고정하되, image provenance와 Docker Compose runtime 검증이 완료되기 전에는 Phase 1을 `VERIFIED`로 올리지 않는다.

## 통합 지점과 실패 경로

Loan Service가 신청 Event를 발행하고 Governance가 Case를 생성한다. Mock 결과 후 Governance가 Decision Command를 발행하며 Loan Service가 멱등 적용한다. 중복·지연·실패 시 기존 상태를 유지하고 재처리 또는 DLT 대상으로 기록한다.

## 완료 기준

- 계약·단위·통합 테스트와 Docker Compose 실행 통과
- `신청 → Case → Mock 평가 → 최종 Loan 상태` 재현
- 중복 Event가 중복 최종 결정을 만들지 않음
- 신청부터 최종 상태까지 최소 Case Timeline 조회 가능
- 문서와 Cross-Repo Baseline 갱신

## 다음 Phase 인계 조건

Mock Agent를 실제 Loan Decision Agent로 교체할 Versioned Snapshot과 평가 요청·결과 계약이 준비되어야 한다. Phase 2는 Phase 1 image provenance와 runtime E2E가 PASS로 기록되어 Phase 1이 `VERIFIED`가 된 뒤 `READY`로 전환한다.

## 실행 추적

- [Plan](plan.md)
- [Progress](progress.md)
- [Cross-Repo Baseline](cross-repo-baseline.md)
- [Verification](verification.md)
- [Troubleshooting](troubleshooting/README.md)
- [Evidence](evidence/README.md)
