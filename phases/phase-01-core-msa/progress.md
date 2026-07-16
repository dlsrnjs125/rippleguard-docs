# Phase 1 Progress

| 작업 | 상태 | 비고 |
| --- | --- | --- |
| Phase 1 Kickoff | IN_PROGRESS | Phase 0 Published Baseline 확인 후 kickoff tracking PR 작성 중 |
| Contracts | NOT_STARTED | Phase 1 신청 REST, Snapshot, Event, Mock Evaluation 계약 확정 예정 |
| Loan Service | NOT_STARTED | 신청, Versioned Snapshot, Outbox, 최종 Loan 상태 반영 구현 예정 |
| Governance Service | NOT_STARTED | Decision Case, Mock Evaluation, Decision Command 구현 예정 |
| Audit Foundation | NOT_STARTED | 원본 Event 저장과 최소 Case Timeline 구현 예정 |
| Infra Integration | NOT_STARTED | 서비스별 PostgreSQL, Kafka, Docker Compose E2E 통합 예정 |
| Cross-Repo Verification | NOT_STARTED | 각 Repository main 병합 후 통합 검증 예정 |
| Final Baseline | NOT_STARTED | Phase 1 완료 시 병합 commit, image, migration, contract version 고정 예정 |

Phase 1 진행은 한 번에 하나의 Repository만 수정한다. 현재 Repository PR이 main에 병합된 뒤 다음 Repository 작업을 시작한다.
