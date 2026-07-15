# Evaluation Run

## 목적과 소유권

Decision Case는 하나의 대출 신청에 대한 Governance 업무 단위다. Evaluation Run은 특정 Snapshot, Model, Prompt, Tool, Agent, Policy Version 조합으로 수행한 한 번의 평가이며 Governance Service가 Run Metadata와 상태를 소유한다. Agent Runtime의 개별 Agent Run은 Evaluation Run에 종속된다.

## 핵심 식별자

| 필드 | 의미 |
| --- | --- |
| `evaluationRunId` | 재실행마다 새로 발급되는 불변 ID |
| `decisionCaseId` | 상위 Governance Case |
| `inputSnapshotVersion` | 평가한 금융·FDS·문서 Reference 조합 |
| `executionPlanVersion` | Agent Trigger와 실행 순서 |
| `modelPromptToolVersions` | 모든 Agent 실행 provenance |
| `policyInputVersion`, `policyBundleVersion` | Assurance와 Routing 재현 기준 |
| `supersedesRunId` | 원인 제거 후 재산출할 때 이전 Run Reference |

## 상태

| 상태 | 의미 | 대표 전이 |
| --- | --- | --- |
| `CREATED` | 고정 입력과 실행 계획으로 생성 | `RUNNING`, `CANCELLED` |
| `RUNNING` | Preflight·Agent·Assurance 평가 중 | `COMPLETED`, `BLOCKED`, `FAILED` |
| `COMPLETED` | Policy 결과와 Trace 생성 완료 | 없음 |
| `BLOCKED` | 현재 Run의 입력·행동에 Hard Block 발생 | 없음; 새 Run만 생성 가능 |
| `FAILED` | 기술·계약·Health 실패 | 없음; 원인 처리 후 새 Run만 생성 가능 |
| `CANCELLED` | 실행 전 또는 실행 중 안전하게 취소 | 없음 |

Run 상태는 덮어쓰거나 `BLOCKED → RUNNING`으로 되돌리지 않는다. 원인이 제거되면 Decision Case를 `RECALCULATION_REQUIRED`로 전환하고 새 Snapshot과 새 `evaluationRunId`로 후속 Run을 생성한다.

## Hard Block 재산출 예시

1. Evaluation Run 1이 금지된 FDS Feature 사용을 탐지해 `BLOCKED`가 된다.
2. Decision Case는 `BLOCKED → RECALCULATION_REQUIRED`로 이동한다.
3. Loan Service가 금지 Feature를 제외한 Versioned Snapshot을 저장한다.
4. Governance Service가 원인 제거를 결정론적으로 확인하고 Evaluation Run 2를 생성한다.
5. Run 2는 Preflight부터 Agent, Assurance, OPA를 모두 재실행한다.

사람은 기존 위반을 Override하지 못한다. 이전 Run, Evidence, Policy Result와 전파 영향은 Audit Trace에 영구 Reference로 남긴다. 실행 가능한 Schema는 `rippleguard-contracts`가 Source of Truth다.
