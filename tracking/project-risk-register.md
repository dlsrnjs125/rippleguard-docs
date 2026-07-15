# Project Risk Register

개별 버그가 아니라 여러 Phase 또는 프로젝트 완료 기준을 흔들 수 있는 위험을 추적한다. 해결된 위험도 삭제하지 않고 상태와 근거를 갱신한다.

| Risk ID | 위험 | 영향 Phase | 가능성 | 영향도 | Owner Repository | Trigger / Detection Signal | 대응 | Mitigation Evidence | Review Phase | 상태 | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RISK-001 | 서비스 간 계약 변경 충돌 | 전체 | 중간 | 높음 | `rippleguard-contracts` | Producer/Consumer Schema·Version 불일치 | Contract First, Consumer 선행, Version Matrix | TBD | 매 Phase | OPEN | - |
| RISK-002 | Agent 출력 재현 불가 | 2~9 | 중간 | 높음 | `rippleguard-agent-runtime` | 동일 Manifest Replay 결과 차이 | Snapshot·Model·Prompt·Tool Version 고정 | TBD | 2, 7, 9 | OPEN | - |
| RISK-003 | 실제 금융 데이터 부재 | 2, 9 | 높음 | 중간 | `rippleguard-docs` | 합성/실제 분포 차이 검증 불가 | 합성 데이터 한계와 일반화 불가 명시 | [MVP Scope](../foundation/mvp-scope.md) | 9, 10 | ACCEPTED | 실제 성능으로 해석하지 않음 |
| RISK-004 | MSA 범위 과대화 | 전체 | 높음 | 높음 | `rippleguard-docs` | Phase 제외 범위 밖 기능 유입 | Phase별 제외 범위와 변경 금지 경계 | [Phase Plans](../phases/README.md) | 매 Phase | MITIGATED | - |
| RISK-005 | LLM Agent가 정책을 대신 판단 | 3~5 | 중간 | 높음 | `rippleguard-governance-service` | 명시 필드 판단이 Agent Output에만 의존 | Preflight·OPA와 Agent 권한 분리 | [Agent Governance](../architecture/agent-governance.md) | 3, 5 | MITIGATED | - |
| RISK-006 | Golden Case Leakage | 3~9 | 높음 | 높음 | `rippleguard-docs` | Holdout 결과를 본 뒤 Prompt·Policy 수정 | Development·Regression·Holdout 분리와 접근 기록 | TBD | 3, 9 | OPEN | - |
| RISK-007 | Agent Common-Mode Failure | 3~5, 9 | 중간 | 높음 | `rippleguard-agent-runtime` | 독립 Agent가 같은 오류 근거·결론 생성 | 독립 Context, Sample Audit, 비교 평가 | TBD | 5, 9 | OPEN | - |
| RISK-008 | FDS Simulator 단순화로 성능 과대평가 | 1~3, 9 | 높음 | 중간 | `rippleguard-infra` | Fixture 다양성·오류가 실제 Source보다 낮음 | REST Mock 오류·지연·Metadata 변형 Case 포함 | TBD | 3, 9 | OPEN | - |
| RISK-009 | Parser 오류를 Evidence Agent 오류로 오인 | 4, 9 | 중간 | 높음 | `rippleguard-agent-runtime` | 추출 결과와 Agent 판단 Error가 분리되지 않음 | Parser Ground Truth와 Agent Metric 분리 | TBD | 4, 9 | OPEN | - |

허용 상태는 `OPEN`, `MITIGATED`, `ACCEPTED`, `CLOSED`다. 상태 변경에는 `Mitigation Evidence`와 `Notes`에 날짜·검증 근거·관련 ADR을 기록한다. `MITIGATED`는 위험이 사라졌다는 뜻이 아니며 Release Readiness에서 잔여 위험을 다시 검토한다.
