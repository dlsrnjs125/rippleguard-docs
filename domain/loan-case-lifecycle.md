# Loan Case Lifecycle

| 상태 | 의미 | 대표 허용 전이 |
| --- | --- | --- |
| `DRAFT` | 작성 중인 신청 | `SUBMITTED` |
| `SUBMITTED` | 접수 완료 | `PREFLIGHT_COMPLETED`, `CLOSED` |
| `PREFLIGHT_COMPLETED` | 형식·필수 자료 사전 점검 완료 | `AGENT_EVALUATION_REQUESTED`, `HUMAN_VERIFICATION_REQUIRED` |
| `AGENT_EVALUATION_REQUESTED` | Agent 평가 요청됨 | `LOAN_PROPOSAL_READY`, `HUMAN_VERIFICATION_REQUIRED` |
| `LOAN_PROPOSAL_READY` | 대출 제안 준비됨 | `ASSURANCE_EVALUATED`, `EVIDENCE_REQUIRED` |
| `ASSURANCE_EVALUATED` | Assurance와 정책 평가 완료 | `EVIDENCE_REQUIRED`, `HUMAN_VERIFICATION_REQUIRED`, `AUTO_DECISION_READY`, `BLOCKED_OR_RECALCULATION` |
| `EVIDENCE_REQUIRED` | 증거 보완 필요 | `ASSURANCE_EVALUATED`, `HUMAN_VERIFICATION_REQUIRED`, `CLOSED` |
| `HUMAN_VERIFICATION_REQUIRED` | 사람 확인 필요 | `ASSURANCE_EVALUATED`, `BLOCKED_OR_RECALCULATION`, `FINALIZED` |
| `AUTO_DECISION_READY` | 자동 결정 조건 충족 | `FINALIZED`, `BLOCKED_OR_RECALCULATION` |
| `BLOCKED_OR_RECALCULATION` | 차단 또는 재계산 필요 | `AGENT_EVALUATION_REQUESTED`, `HUMAN_VERIFICATION_REQUIRED`, `CLOSED` |
| `FINALIZED` | 최종 대출 상태 반영 완료 | `CLOSED` |
| `CLOSED` | 업무 종료 | 없음 |

모든 Case가 모든 상태를 순차 통과하지는 않는다. 위험, 증거 수준, 운영 모드에 따라 안전한 분기와 재평가가 가능하다. 다만 `DRAFT`에서 자동 결정으로 건너뛰거나, `ASSURANCE_EVALUATED` 없이 `AUTO_DECISION_READY`로 이동하거나, `CLOSED`를 재개하거나, Agent Runtime이 `FINALIZED`를 직접 설정하는 전이는 금지한다. 상세 전이 계약과 명령 Schema는 `rippleguard-contracts`에서 확정한다.
