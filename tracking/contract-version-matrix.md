# Contract Version Matrix

| 계약 | Producer | Consumer | Schema Version | 호환 범위 | 검증 결과 | 상태 |
| --- | --- | --- | --- | --- | --- | --- |
| External Risk Signal | FDS Simulator | Loan Service | `1.0.0` | Phase 0 schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Loan Application REST | Web | Loan Service | TBD | TBD | REST/OpenAPI not implemented in Phase 0 | NOT_STARTED |
| Loan Application Status | Loan Service | Governance Service | `1.0.0` | Phase 0 domain schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Decision Case Status | Governance Service | Loan/Governance consumers | `1.0.0` | Phase 0 domain schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| `loan.application.submitted` | Loan Service | Governance, Audit | `eventType: loan.application.submitted.v1`; schema `1.0.0`, compatible minor `1.1.0` | same major `v1` | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| `agent.evaluation.requested` | Governance Service | Agent Runtime | `eventType: agent.evaluation.requested.v1`; schema `1.0.0` | same major `v1` | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| `agent.evaluation.completed` | Agent Runtime | Governance, Audit | `eventType: agent.evaluation.completed.v1`; schema `1.0.0` | same major `v1` | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| `loan.decision.commanded` | Governance Service | Loan Service | `eventType: loan.decision.commanded.v1`; schema `1.0.0` | same major `v1` | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Decision Envelope | Agent Runtime | Governance Service | `1.0.0` | Phase 0 schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Evaluation Run | Governance Service | Agent Runtime, Audit Service | `1.0.0` | Phase 0 schema | `make validate` PASS in `rippleguard-contracts` | CONTRACT_READY |
| Consequence Envelope | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Evidence & Control Findings | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Assurance Policy Input | Governance Service | OPA | TBD | TBD | TBD | NOT_STARTED |
| Sample Audit Escape Event | Audit/Replay Service | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Impacted Evaluation Run Query | Governance Service | Audit/Replay Service | TBD | TBD | TBD | NOT_STARTED |

`CONTRACT_READY`는 실행 가능한 Schema가 준비됐다는 뜻이며 서비스 구현 완료를 의미하지 않는다. 후속 Phase 계약은 `NOT_STARTED`로 유지한다.
