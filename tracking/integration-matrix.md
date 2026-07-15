# Integration Matrix

| Producer | Consumer | 통신 | 계약 | 목적 | 구현 상태 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator | Loan Service | REST 또는 합성 Fixture | External Risk Signal TBD | 목적·범위가 있는 위험신호 Snapshot | NOT_STARTED |
| Loan Service | Governance Service | Kafka | `loan.application.submitted.v1` | Decision Case 생성 | NOT_STARTED |
| Governance Service | Loan Service | Kafka | `governance.review.started.v1` | Loan Application을 검토 중으로 전이 | NOT_STARTED |
| Governance Service | Loan Service | Kafka | `governance.evidence.requested.v1` | 한정된 Evidence 보완 요청 | NOT_STARTED |
| Loan Service | Governance Service | Kafka | `loan.evidence.updated.v1` | Versioned Snapshot 재평가 | NOT_STARTED |
| Governance Service | Agent Runtime | Kafka | `agent.evaluation.requested.v1` | Agent 평가 요청 | NOT_STARTED |
| Agent Runtime | Governance Service | Kafka | `agent.evaluation.completed.v1` | Agent 결과 반환 | NOT_STARTED |
| Governance Service | Loan Service | Kafka | `loan.decision.commanded.v1` | 검증된 최종 처리 명령 | NOT_STARTED |
| Loan Service | Governance Service | Kafka | `loan.decision.finalized.v1` | 최종 반영 결과 통지 | NOT_STARTED |
| Governance Service | OPA | 동기 API | Policy Input/Decision TBD | Assurance 정책 평가 | NOT_STARTED |
| Web | Loan Service | REST | OpenAPI TBD | 신청과 대출 상태 조회 | NOT_STARTED |
| Web | Governance Service | REST | OpenAPI TBD | Case·Agent 운영 | NOT_STARTED |
| Web | Audit & Replay Service | REST | OpenAPI TBD | Trace·Replay 조회 | NOT_STARTED |

계약 이름은 통합 지점을 계획하기 위한 식별자이며 아직 구현된 Schema를 의미하지 않는다. 최종 원본과 버전은 `rippleguard-contracts`에서 확정한다.
