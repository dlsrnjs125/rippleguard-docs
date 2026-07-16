# Canonical Scenarios

MVP 전체를 관통하는 대표 흐름의 이름과 기대 경로를 고정한다. 이 표는 설계·Demo 기준이며 실행 가능한 Fixture와 기대 결과의 Source of Truth는 각 Phase의 Versioned Dataset과 `rippleguard-contracts`다.

| ID | 시나리오 | 핵심 입력 | 기대 Agent | 기대 Assurance | 최종 경로 | 검증 Phase |
| --- | --- | --- | --- | --- | --- | --- |
| CS-001 | 정상 자동 승인 | 안정적 소득, 허용 데이터, 증빙·통제 완료 | Loan Decision; 전문 Agent는 Trigger상 생략 또는 Sample Audit | `COMPLETE`, Hard Block 없음 | `SAFE_AUTOMATION → APPROVE` | 2, 5, 9 |
| CS-002 | 명확한 대출 정책상 거절 | 높은 부채·연체 등 허용된 Credit Feature | Loan Decision | `COMPLETE`, Governance 위반 없음 | `SAFE_AUTOMATION → REJECT` | 2, 5, 9 |
| CS-003 | FDS Cross-Purpose Reuse | Fraud 목적 FDS Feature가 Credit 평가에 포함 | RippleGuard 필수 | `VIOLATED` | 현재 Run `HARD_BLOCK`, Feature 제거 후 재산출 가능 | 3, 5, 9 |
| CS-004 | 거래→신청자 Scope Escalation | 단일 거래 이상을 신청자 전체 위험으로 확대 | RippleGuard 필수 | `VIOLATED` | 현재 Run `HARD_BLOCK`, 범위 제한 Snapshot 재산출 | 3, 5, 9 |
| CS-005 | 미확정 FDS 추론 사실화 | `SUSPECTED` 신호를 확정 고객 위험으로 설명 | RippleGuard 필수 | `VIOLATED` | 현재 Run `HARD_BLOCK`, 정정 Snapshot 재산출 | 3, 5, 9 |
| CS-006 | 프리랜서 소득 Legacy Control 누락 | 필수 소득 확인 절차·문서 누락 | Evidence & Control 필수 | `INCOMPLETE` | `ADDITIONAL_VERIFICATION`, 보완 후 새 Run | 4, 5, 9 |
| CS-007 | 문서·정형 데이터 충돌 | DB 소득과 정산명세서 불일치 | Evidence & Control 필수; 필요 시 RippleGuard | `INCOMPLETE` | 사실 확인 후 새 Snapshot·새 Run | 4, 5, 9 |
| CS-008 | Agent 장애와 Manual Safe Mode | 필수 Agent Timeout·Health 불가 | 계획된 필수 Agent 실행 실패 | `INCOMPLETE` | `MANUAL_SAFE_MODE`, 복구 후 새 Run과 Replay | 7, 8, 9 |

각 Scenario는 Development, Regression, Holdout 중 Dataset Partition을 명시한다. 같은 의미를 가진 Holdout Case의 세부 값과 기대 결과를 Prompt·Policy 튜닝에 사용하지 않는다.

## Graph Demonstration Mapping

Graph 시연은 대표 Case의 핵심 인과관계만 보여준다. 학습 데이터 전체, 문서 원문, 원문 Prompt, 전체 금융 Snapshot은 Graph Node로 표시하지 않는다.

| Scenario | Architecture Graph 관련 여부 | Execution Graph 예상 핵심 Node | 예상 최종 Routing | 시연 Phase |
| --- | --- | --- | --- | --- |
| CS-001 정상 자동 승인 | Loan Service, Governance Service, Agent Runtime, Kafka, OPA 기본 경로 | Case, EvaluationRun, Loan Decision Agent, Decision Envelope, Policy Decision, Final Decision | `SAFE_AUTOMATION → APPROVE` | 6, 7, 9 |
| CS-003 Cross-Purpose Data Reuse Hard Block | FDS Simulator, Loan Service, RippleGuard Agent, OPA 관계 | Case, EvaluationRun, FDS Event, RippleGuard Agent Run, Consequence Envelope, Policy Decision, Hard Block | `HARD_BLOCK` | 6, 7, 9 |
| CS-004 Scope Escalation 탐지 | FDS Simulator, RippleGuard Agent, Governance Service 관계 | Case, EvaluationRun, Scope Metadata Event, RippleGuard Agent Run, Consequence Envelope, Policy Decision | `HARD_BLOCK` | 6, 7, 9 |
| CS-006 Evidence 누락 후 Human Verification | Evidence & Control Agent, Human Task, Governance Service 관계 | Case, EvaluationRun, Evidence Finding, Human Verification Task, Policy Decision | `ADDITIONAL_VERIFICATION` | 6, 7, 9 |
| CS-003/CS-004/CS-005 수정 Snapshot 기반 새 EvaluationRun 재산출 | Loan Service Snapshot, Governance Service, Audit & Replay Service 관계 | Previous EvaluationRun, `supersedesRunId`, New EvaluationRun, corrected Snapshot Reference, Final Decision | 원인 제거 후 새 Run으로 재평가 | 7, 9 |
| CS-008 Agent 장애 후 Manual Safe Mode | Agent Runtime, Governance Service, Web, Audit & Replay Service 관계 | Agent Run Timeout, EvaluationRun Failed/Incomplete, Manual Safe Mode Event, Human Task | `MANUAL_SAFE_MODE` | 7, 8, 9 |
