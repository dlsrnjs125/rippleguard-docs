# Integration Matrix

| Producer | Consumer | 통신 | 계약 | 목적 | 구현 상태 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator Container | Loan Service | REST | External Risk Signal `1.0.0` | 운영형 Demo의 목적·범위 Risk Signal을 Versioned Snapshot으로 저장 | CONTRACT_READY |
| Loan Service | Governance Service | Kafka | `loan.application.submitted.v1` | Decision Case 생성 | CONTRACT_READY |
| Governance Service | Loan Service | Kafka | `governance.review.started.v1` | Loan Application을 검토 중으로 전이 | CONTRACT_READY |
| Governance Service | Loan Service | Kafka | `governance.evidence.requested.v1` | 한정된 Evidence 보완 요청 | CONTRACT_READY |
| Loan Service | Governance Service | Kafka | `loan.evidence.updated.v1` | Versioned Snapshot 재평가 | CONTRACT_READY |
| Governance Service | Agent Runtime | Kafka | `agent.evaluation.requested.v1` | Agent 평가 요청 | CONTRACT_READY |
| Agent Runtime | Governance Service | Kafka | `agent.evaluation.completed.v1` | Agent 결과 반환 | CONTRACT_READY |
| Governance Service | Loan Service | Kafka | `loan.decision.commanded.v1` | 검증된 최종 처리 명령 | CONTRACT_READY |
| Loan Service | Governance Service | Kafka | `loan.decision.finalized.v1` | 최종 반영 결과 통지 | CONTRACT_READY |
| Governance Service | OPA | 동기 API | Policy Input/Decision TBD | Assurance 정책 평가 | NOT_STARTED |
| Web | Loan Service | REST | OpenAPI TBD | 신청과 대출 상태 조회 | NOT_STARTED |
| Web | Governance Service | REST | OpenAPI TBD | Case·Agent 운영 | NOT_STARTED |
| Web | Audit & Replay Service | REST | OpenAPI TBD | Trace·Replay 조회 | NOT_STARTED |

`CONTRACT_READY`는 `rippleguard-contracts`와 `rippleguard-infra`의 topic/baseline이 준비됐다는 뜻이다. 서비스 구현은 아직 없으므로 `IMPLEMENTED` 또는 `VERIFIED`로 표시하지 않는다.
