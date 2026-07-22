# Integration Matrix

| Producer | Consumer | 통신 | 계약 | 목적 | 구현 상태 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator Container | Loan Service | REST | External Risk Signal payload schema `1.0.0`; REST contract TBD | 운영형 Demo의 목적·범위 Risk Signal을 Versioned Snapshot으로 저장 | OUT_OF_SCOPE_FOR_PHASE |
| Loan Service | Governance Service, Audit Replay Service | Kafka | `loan.application.submitted.v1` | Decision Case 생성과 Audit Timeline 기록 | RUNTIME_VERIFIED |
| Governance Service | Loan Service, Audit Replay Service | Kafka | `governance.review.started.v1` | Loan Application을 검토 중으로 전이하고 Audit Timeline에 기록 | RUNTIME_VERIFIED |
| Governance Service | Loan Service | Kafka | `governance.evidence.requested.v1` | 한정된 Evidence 보완 요청 | CONTRACT_READY / GOVERNANCE_PRODUCER_DEFERRED |
| Loan Service | Governance Service | Kafka | `loan.evidence.updated.v1` | Versioned Snapshot 재평가 | PARTIALLY_IMPLEMENTED |
| Governance Service | Audit Replay Service | Kafka | `agent.evaluation.requested.v1` | Phase 1 Mock Evaluation 요청 사실을 Audit Timeline에 기록 | RUNTIME_VERIFIED |
| Governance Service | Audit Replay Service | Kafka | `agent.evaluation.completed.v1` | Phase 1 Mock Evaluation result를 Audit Timeline에 기록 | RUNTIME_VERIFIED |
| Governance Service | Agent Runtime | Kafka or API TBD | Agent Request Contract TBD | Fixed Snapshot and model-version Loan Proposal request | PLANNED |
| Agent Runtime | Governance Service | REST API | Loan Proposal / Phase 2 Decision Envelope | Validated proposal or explicit failure response; not final Loan state mutation | BLOCKED_BY_FEATURE_SNAPSHOT_PATH |
| Governance Service | Audit Replay Service | Kafka TBD | Agent Result Validated/Rejected Event TBD | Governance-validated Agent result or rejection recorded after output validation | PLANNED |
| Agent Runtime | Local LLM Runtime | HTTP via `StructuredLlmPort` | Structured LLM Request TBD | Phase 3·4 Local LLM 추론 호출 | PLANNED |
| Local LLM Runtime | Agent Runtime | HTTP Provider Response | Schema-constrained Response TBD | 구조화 LLM 응답 반환 | PLANNED |
| Agent Runtime | Governance Service | Kafka | Agent Output Event TBD | Local LLM Agent 결과 검증과 실행 경로 결정 | PLANNED |
| Agent Runtime | Audit Replay Service | None for normal Phase 2 result path | Agent Runtime direct Audit publication is forbidden | Governance emits `governance.agent-result.validated.v1`; Audit consumes Governance event only | NOT_APPLICABLE_PHASE2 |
| Governance Service | Loan Service, Audit Replay Service | Kafka | `loan.decision.commanded.v1` | 검증된 최종 처리 명령과 Audit Timeline 기록 | RUNTIME_VERIFIED |
| Loan Service | Audit Replay Service | Kafka | `loan.decision.finalized.v1` | 최종 반영 결과를 Audit Timeline에 기록 | RUNTIME_VERIFIED |
| Governance Service | OPA | 동기 API | Policy Input/Decision TBD | Assurance 정책 평가 | OUT_OF_SCOPE_FOR_PHASE |
| Web | Loan Service | REST | OpenAPI TBD | 신청과 대출 상태 조회 | OUT_OF_SCOPE_FOR_PHASE |
| Web | Governance Service | REST | OpenAPI TBD | Case·Agent 운영 | OUT_OF_SCOPE_FOR_PHASE |
| Web | Audit Replay Service | REST | OpenAPI TBD | Trace·Replay 조회 | OUT_OF_SCOPE_FOR_PHASE |
| `rippleguard-infra` | Web | Static artifact / REST TBD | Topology Manifest Schema and Graph Node·Link DTO TBD | Versioned Topology Manifest를 배포하고 Architecture Graph 렌더링에 사용 | PLANNED |
| Audit Replay Service | Web | REST | EvaluationRun Graph Read API TBD | Case·EvaluationRun Execution Graph 조회 | PLANNED |
| Governance Service | Audit Replay Service | Kafka / REST TBD | Agent Registry·EvaluationRun Metadata Event TBD | Graph Read Model에 Agent Registry와 Run Metadata 제공 | PLANNED |
| Contracts | Infra / Web / Audit Replay Service | Schema / OpenAPI TBD | Topology Manifest Schema, Graph Node·Link Core DTO, Architecture·Execution Extension DTO TBD | Graph Payload와 조회 API 계약 | PLANNED |

## Status Definitions

- `CONTRACT_READY`: Contract exists and validates, but implementation is not claimed.
- `PARTIALLY_IMPLEMENTED`: At least one side of the producer/consumer path is implemented, but the full path is not implemented or verified.
- `IMPLEMENTED_PENDING_E2E`: Producer and consumer implementation exist and service-level tests passed, but Docker Compose runtime E2E is not recorded as PASS.
- `RUNTIME_VERIFIED`: Producer and consumer implementation exist and Phase 1 Docker Compose runtime verification passed.
- `BLOCKED`: Work cannot proceed until an upstream prerequisite is resolved.
- `OUT_OF_SCOPE_FOR_PHASE`: Not part of the Phase 1 core vertical flow.
- `PLANNED`: Future contract or implementation planning only.

The matrix must be compared with the `rippleguard-infra` Phase 1 manifest before any Phase status transition. Contract existence does not imply producer implementation, consumer implementation or integrated E2E verification.
