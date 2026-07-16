# Phase 1 Progress

| 작업 | 상태 | 비고 |
| --- | --- | --- |
| Phase 1 Kickoff | IN_REVIEW | Phase 0 Published Baseline 확인 후 kickoff tracking PR #6 review 중 |
| Contracts | NOT_STARTED | 신청 REST·Snapshot 신규 계약과 Phase 0 Event·Mock Evaluation 적용 계약 보완 예정 |
| Loan Service | NOT_STARTED | 신청, Versioned Snapshot, Outbox, 최종 Loan 상태 반영 구현 예정 |
| Governance Service | NOT_STARTED | Decision Case, Mock Evaluation, Decision Command 구현 예정 |
| Audit Foundation | NOT_STARTED | 원본 Event 저장과 최소 Case Timeline 구현 예정 |
| Infra Integration | NOT_STARTED | 서비스별 PostgreSQL, Kafka, Docker Compose E2E 통합 예정 |
| Cross-Repo Verification | NOT_STARTED | 각 Repository main 병합 후 통합 검증 예정 |
| Final Baseline | NOT_STARTED | Phase 1 완료 시 병합 commit, image, migration, contract version 고정 예정 |

Phase 1 진행은 한 번에 하나의 Repository만 수정한다. 현재 Repository PR이 main에 병합된 뒤 다음 Repository 작업을 시작한다.

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
