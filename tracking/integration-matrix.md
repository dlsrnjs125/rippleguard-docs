# Integration Matrix

| Producer | Consumer | 통신 | 계약 | 목적 | 구현 상태 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator Container | Loan Service | REST | External Risk Signal payload schema `1.0.0`; REST contract TBD | 운영형 Demo의 목적·범위 Risk Signal을 Versioned Snapshot으로 저장 | NOT_STARTED |
| Loan Service | Governance Service | Kafka | `loan.application.submitted.v1` | Decision Case 생성 | CONTRACT_READY |
| Governance Service | Loan Service | Kafka | `governance.review.started.v1` | Loan Application을 검토 중으로 전이 | CONTRACT_READY |
| Governance Service | Loan Service | Kafka | `governance.evidence.requested.v1` | 한정된 Evidence 보완 요청 | CONTRACT_READY |
| Loan Service | Governance Service | Kafka | `loan.evidence.updated.v1` | Versioned Snapshot 재평가 | CONTRACT_READY |
| Governance Service | Agent Runtime | Kafka | `agent.evaluation.requested.v1` | Agent 평가 요청 | CONTRACT_READY |
| Governance Service | Governance Service / Internal Kafka | Kafka | `agent.evaluation.completed.v1` | Phase 1 Mock evaluation result | CONTRACT_READY |
| Agent Runtime | Governance Service | Kafka | `agent.evaluation.completed.v1` | Phase 2+ actual Agent result | CONTRACT_READY, implementation deferred |
| Governance Service | Loan Service | Kafka | `loan.decision.commanded.v1` | 검증된 최종 처리 명령 | CONTRACT_READY |
| Loan Service | Governance Service | Kafka | `loan.decision.finalized.v1` | 최종 반영 결과 통지 | CONTRACT_READY |
| Governance Service | OPA | 동기 API | Policy Input/Decision TBD | Assurance 정책 평가 | NOT_STARTED |
| Web | Loan Service | REST | OpenAPI TBD | 신청과 대출 상태 조회 | NOT_STARTED |
| Web | Governance Service | REST | OpenAPI TBD | Case·Agent 운영 | NOT_STARTED |
| Web | Audit & Replay Service | REST | OpenAPI TBD | Trace·Replay 조회 | NOT_STARTED |
| `rippleguard-infra` | Web | Static artifact / REST TBD | Topology Manifest Schema and Graph Node·Link DTO TBD | Versioned Topology Manifest를 배포하고 Architecture Graph 렌더링에 사용 | PLANNED |
| Audit & Replay Service | Web | REST | EvaluationRun Graph Read API TBD | Case·EvaluationRun Execution Graph 조회 | PLANNED |
| Governance Service | Audit & Replay Service | Kafka / REST TBD | Agent Registry·EvaluationRun Metadata Event TBD | Graph Read Model에 Agent Registry와 Run Metadata 제공 | PLANNED |
| Contracts | Infra / Web / Audit & Replay Service | Schema / OpenAPI TBD | Topology Manifest Schema, Graph Node·Link Core DTO, Architecture·Execution Extension DTO TBD | Graph Payload와 조회 API 계약 | PLANNED |

`CONTRACT_READY`는 `rippleguard-contracts`와 `rippleguard-infra`의 topic/baseline이 준비됐다는 뜻이다. `PLANNED`는 향후 계약 또는 구현 계획만 있음을 뜻한다. 서비스 구현은 아직 없으므로 `IMPLEMENTED` 또는 `VERIFIED`로 표시하지 않는다. External Risk Signal 객체 Schema는 준비됐지만 FDS Simulator REST 통합은 Phase 0 범위 밖이므로 `NOT_STARTED`로 유지한다.
