# Loan Application and Decision Case Lifecycles

Loan Service와 Governance Service는 서로 다른 DB와 상태를 소유한다. 두 상태를 하나의 분산 상태 머신처럼 공동 갱신하지 않으며, 계약된 Event를 통해 독립적으로 전이한다.

## Loan Application Status

Loan Service만 이 상태를 변경한다.

| 상태 | 의미 | 대표 허용 전이 |
| --- | --- | --- |
| `DRAFT` | 작성 중인 신청 | `SUBMITTED` |
| `SUBMITTED` | 신청 접수와 Snapshot 저장 완료 | `UNDER_GOVERNANCE_REVIEW`, `CLOSED` |
| `UNDER_GOVERNANCE_REVIEW` | Governance 검토 진행 중 | `EVIDENCE_REQUIRED`, `DECISION_RECEIVED`, `CLOSED` |
| `EVIDENCE_REQUIRED` | 신청자 또는 검토자의 증빙 보완 필요 | `UNDER_GOVERNANCE_REVIEW`, `CLOSED` |
| `DECISION_RECEIVED` | 검증된 Governance 명령 수신 | `FINALIZED` |
| `FINALIZED` | 최종 대출 결과 반영 완료 | `CLOSED` |
| `CLOSED` | 신청 업무 종료 | 없음 |

## Decision Case Status

Governance Service만 이 상태를 변경한다.

| 상태 | 의미 | 대표 허용 전이 |
| --- | --- | --- |
| `CREATED` | 신청 Event로 Decision Case 생성 | `PREFLIGHT_COMPLETED`, `VERIFICATION_REQUIRED`, `BLOCKED` |
| `PREFLIGHT_COMPLETED` | 결정론적 입력·목적·필수 통제 사전 점검 완료 | `EVALUATION_REQUESTED`, `VERIFICATION_REQUIRED`, `BLOCKED` |
| `EVALUATION_REQUESTED` | Governance가 실행 계획을 확정하고 Agent 평가 요청 | `PROPOSAL_READY`, `VERIFICATION_REQUIRED`, `BLOCKED` |
| `PROPOSAL_READY` | Decision Envelope와 필요한 전문 Agent 결과 수신 | `ASSURANCE_EVALUATED`, `VERIFICATION_REQUIRED`, `RECALCULATION_REQUIRED` |
| `ASSURANCE_EVALUATED` | Assurance Profile과 정책 평가 완료 | `VERIFICATION_REQUIRED`, `BLOCKED`, `RECALCULATION_REQUIRED`, `RESOLVED` |
| `VERIFICATION_REQUIRED` | 확정되지 않은 사실 또는 증빙 보완 필요 | `PREFLIGHT_COMPLETED`, `EVALUATION_REQUESTED`, `BLOCKED` |
| `BLOCKED` | 현재 Evaluation Run의 금지 조건 또는 안전 경로 실패로 진행 차단 | `RECALCULATION_REQUIRED`, `RESOLVED` |
| `RECALCULATION_REQUIRED` | 정형 입력 변경 후 재평가 필요 | `PREFLIGHT_COMPLETED`, `EVALUATION_REQUESTED`, `BLOCKED` |
| `RESOLVED` | Governance 실행 경로 확정 | 없음 |

## Event 연결

| Event | 발행 주체 | Loan Application 전이 | Decision Case 전이 |
| --- | --- | --- | --- |
| `loan.application.submitted.v1` | Loan Service | `DRAFT → SUBMITTED` 후 발행 | 수신 시 `CREATED` 생성 |
| `governance.review.started.v1` | Governance Service | `SUBMITTED → UNDER_GOVERNANCE_REVIEW` | `CREATED` 유지 또는 Preflight 진행 |
| `governance.evidence.requested.v1` | Governance Service | `UNDER_GOVERNANCE_REVIEW → EVIDENCE_REQUIRED` | `→ VERIFICATION_REQUIRED` |
| `loan.evidence.updated.v1` | Loan Service | `EVIDENCE_REQUIRED → UNDER_GOVERNANCE_REVIEW` | `VERIFICATION_REQUIRED → PREFLIGHT_COMPLETED` 또는 재평가 |
| `loan.decision.commanded.v1` | Governance Service | 수신 시 `UNDER_GOVERNANCE_REVIEW → DECISION_RECEIVED` | 발행 전에 `→ RESOLVED` |
| `loan.decision.finalized.v1` | Loan Service | `DECISION_RECEIVED → FINALIZED` | 상태 변경 없이 결과 참조 기록 |

`BLOCKED → RECALCULATION_REQUIRED`는 정책 위반을 Override하는 전이가 아니다. 금지된 FDS Feature 제거, 만료된 Snapshot 교체처럼 위반 원인이 제거된 새 입력 Version이 검증된 경우에만 허용한다. 이후 새 Evaluation Run을 만들고 `EVALUATION_REQUESTED`로 진행하며 차단된 Run과 근거는 보존한다. 복구할 수 없는 정책 결과나 신청 종료는 `RESOLVED`로 간다.

Event는 원자적 교차 DB 갱신을 의미하지 않는다. 각 Consumer는 중복과 순서 지연을 견디도록 멱등 처리하고, 허용되지 않은 전이는 거부·기록한다. 한 Decision Case 안의 개별 실행 상태는 [Evaluation Run](evaluation-run.md)이 소유한다. 상세 상태와 Event Schema의 Source of Truth는 `rippleguard-contracts`다.
