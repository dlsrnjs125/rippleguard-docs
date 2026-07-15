# Contract Version Matrix

| 계약 | Producer | Consumer | Schema Version | 호환 범위 | 검증 결과 | 상태 |
| --- | --- | --- | --- | --- | --- | --- |
| External Risk Signal | FDS Simulator | Loan Service | TBD | TBD | TBD | NOT_STARTED |
| Loan Application REST | Web | Loan Service | TBD | TBD | TBD | NOT_STARTED |
| `loan.application.submitted` | Loan Service | Governance, Audit | TBD | TBD | TBD | NOT_STARTED |
| `agent.evaluation.requested` | Governance Service | Agent Runtime | TBD | TBD | TBD | NOT_STARTED |
| `agent.evaluation.completed` | Agent Runtime | Governance, Audit | TBD | TBD | TBD | NOT_STARTED |
| `loan.decision.commanded` | Governance Service | Loan Service | TBD | TBD | TBD | NOT_STARTED |
| Decision Envelope | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Consequence Envelope | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Evidence & Control Findings | Agent Runtime | Governance Service | TBD | TBD | TBD | NOT_STARTED |
| Assurance Policy Input | Governance Service | OPA | TBD | TBD | TBD | NOT_STARTED |

계약 구현 전이므로 버전을 추측하지 않는다. 계약이 추가되면 `rippleguard-contracts`의 태그 또는 불변 식별자와 Producer/Consumer 호환성 검증을 기록한다.
