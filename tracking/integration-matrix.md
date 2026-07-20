# Integration Matrix

| Producer | Consumer | 통신 | 계약 | 목적 | 구현 상태 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator Container | Loan Service | REST | External Risk Signal payload schema `1.0.0`; REST contract TBD | 운영형 Demo의 목적·범위 Risk Signal을 Versioned Snapshot으로 저장 | NOT_STARTED |
| Loan Service | Governance Service | Kafka | `loan.application.submitted.v1` | Decision Case 생성 | IMPLEMENTED_PENDING_E2E |
| Governance Service | Loan Service | Kafka | `governance.review.started.v1` | Loan Application을 검토 중으로 전이 | IMPLEMENTED_PENDING_E2E |
| Governance Service | Loan Service | Kafka | `governance.evidence.requested.v1` | 한정된 Evidence 보완 요청 | IMPLEMENTED_PENDING_E2E |
| Loan Service | Governance Service | Kafka | `loan.evidence.updated.v1` | Versioned Snapshot 재평가 | IMPLEMENTED_PENDING_E2E |
| Governance Service | Audit & Agent Runtime | Kafka | `agent.evaluation.requested.v1` | Phase 1 Audit 기록과 Phase 2 Agent 평가 요청 경계 | IMPLEMENTED_PENDING_E2E |
| Governance Service | Governance Service / Audit | Kafka | `agent.evaluation.completed.v1` | Phase 1 Mock evaluation result | IMPLEMENTED_PENDING_E2E |
| Agent Runtime | Governance Service | Kafka | `agent.evaluation.completed.v1` | Phase 2+ actual Agent result | CONTRACT_READY, implementation deferred |
| Governance Service | Loan Service | Kafka | `loan.decision.commanded.v1` | 검증된 최종 처리 명령 | IMPLEMENTED_PENDING_E2E |
| Loan Service | Audit & Governance Service | Kafka | `loan.decision.finalized.v1` | 최종 반영 결과 통지 | IMPLEMENTED_PENDING_E2E |
| Governance Service | OPA | 동기 API | Policy Input/Decision TBD | Assurance 정책 평가 | NOT_STARTED |
| Web | Loan Service | REST | OpenAPI TBD | 신청과 대출 상태 조회 | NOT_STARTED |
| Web | Governance Service | REST | OpenAPI TBD | Case·Agent 운영 | NOT_STARTED |
| Web | Audit & Replay Service | REST | OpenAPI TBD | Trace·Replay 조회 | NOT_STARTED |
| `rippleguard-infra` | Web | Static artifact / REST TBD | Topology Manifest Schema and Graph Node·Link DTO TBD | Versioned Topology Manifest를 배포하고 Architecture Graph 렌더링에 사용 | PLANNED |
| Audit & Replay Service | Web | REST | EvaluationRun Graph Read API TBD | Case·EvaluationRun Execution Graph 조회 | PLANNED |
| Governance Service | Audit & Replay Service | Kafka / REST TBD | Agent Registry·EvaluationRun Metadata Event TBD | Graph Read Model에 Agent Registry와 Run Metadata 제공 | PLANNED |
| Contracts | Infra / Web / Audit & Replay Service | Schema / OpenAPI TBD | Topology Manifest Schema, Graph Node·Link Core DTO, Architecture·Execution Extension DTO TBD | Graph Payload와 조회 API 계약 | PLANNED |

`IMPLEMENTED_PENDING_E2E`는 Producer/Consumer 구현과 service-level tests는 확인됐지만 Docker Compose runtime E2E가 아직 PASS로 기록되지 않았다는 뜻이다. `PLANNED`는 향후 계약 또는 구현 계획만 있음을 뜻한다. External Risk Signal 객체 Schema는 준비됐지만 FDS Simulator REST 통합은 Phase 1 핵심 수직 흐름 밖이므로 `NOT_STARTED`로 유지한다.
