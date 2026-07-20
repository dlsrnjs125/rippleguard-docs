# Contract Version Matrix

| 계약 | Producer | Consumer | Schema Version | 호환 범위 | 검증 결과 | 상태 |
| --- | --- | --- | --- | --- | --- | --- |
| External Risk Signal | FDS Simulator | Loan Service | `1.0.0` | Phase 0 schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Loan Application REST | Web / test client | Loan Service | OpenAPI `phase-1-core-msa.v1.0.0` | `1.0.0` | `make validate` PASS in `rippleguard-contracts`; Loan service tests PASS | IMPLEMENTED_PENDING_E2E |
| Loan Application Status | Loan Service | Governance Service | `1.0.0` | Phase 0 domain schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Decision Case Status | Governance Service | Loan/Governance consumers | `1.0.0` | Phase 0 domain schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| `loan.application.submitted` | Loan Service | Governance, Audit | `eventType: loan.application.submitted.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Loan/Governance/Audit service tests PASS | IMPLEMENTED_PENDING_E2E |
| `governance.review.started` | Governance Service | Loan, Audit | `eventType: governance.review.started.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Loan/Governance service tests PASS | IMPLEMENTED_PENDING_E2E |
| `agent.evaluation.requested` | Governance Service | Audit; Phase 2 Agent Runtime | `eventType: agent.evaluation.requested.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Governance/Audit service tests PASS | IMPLEMENTED_PENDING_E2E |
| `agent.evaluation.completed` | Phase 1 MOCK: Governance Service; Phase 2+ AGENT: Agent Runtime | Governance, Audit | `eventType: agent.evaluation.completed.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Governance/Audit service tests PASS | IMPLEMENTED_PENDING_E2E |
| `loan.decision.commanded` | Governance Service | Loan Service | `eventType: loan.decision.commanded.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Loan/Governance service tests PASS | IMPLEMENTED_PENDING_E2E |
| `loan.decision.finalized` | Loan Service | Audit | `eventType: loan.decision.finalized.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | Contract validation PASS; Loan/Audit service tests PASS | IMPLEMENTED_PENDING_E2E |
| Decision Envelope | Phase 1 MOCK: Governance Service / `mock-evaluator`; Phase 2+ AGENT: Agent Runtime | Governance Service | `1.0.0` | Phase 0 schema | Contract validation PASS; deterministic mock evaluation service tests PASS | IMPLEMENTED_PENDING_E2E |
| Evaluation Run | Governance Service | Agent Runtime, Audit Service | `1.0.0`, `2.0.0` schema available | Phase 1 uses service-local persisted run plus event refs | Contract validation PASS; Governance service tests PASS | IMPLEMENTED_PENDING_E2E |
| Consequence Envelope | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Evidence & Control Findings | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Assurance Policy Input | Governance Service | OPA | TBD | TBD | TBD | NOT_STARTED |
| Sample Audit Escape Event | Audit/Replay Service | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Impacted Evaluation Run Query | Governance Service | Audit/Replay Service | TBD | TBD | TBD | NOT_STARTED |

`IMPLEMENTED_PENDING_E2E`는 contract validation과 service-level tests가 통과했지만 Docker Compose runtime E2E가 아직 PASS로 기록되지 않았다는 뜻이다. 후속 Phase 계약은 `NOT_STARTED`로 유지한다.
