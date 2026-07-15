# Project Risk Register

개별 버그가 아니라 여러 Phase 또는 프로젝트 완료 기준을 흔들 수 있는 위험을 추적한다. 해결된 위험도 삭제하지 않고 상태와 근거를 갱신한다.

| Risk ID | 위험 | 영향 Phase | 가능성 | 영향도 | 대응 | 상태 |
| --- | --- | --- | --- | --- | --- | --- |
| RISK-001 | 서비스 간 계약 변경 충돌 | 전체 | 중간 | 높음 | Contract First, Consumer 선행, Version Matrix | OPEN |
| RISK-002 | Agent 출력 재현 불가 | 2~9 | 중간 | 높음 | Snapshot·Model·Prompt·Tool Version 고정과 Replay | OPEN |
| RISK-003 | 실제 금융 데이터 부재 | 2, 9 | 높음 | 중간 | 합성 데이터 한계와 일반화 불가 명시 | ACCEPTED |
| RISK-004 | MSA 범위 과대화 | 전체 | 높음 | 높음 | Phase별 제외 범위와 변경 금지 경계 | MITIGATED |
| RISK-005 | LLM Agent가 정책을 대신 판단 | 3~5 | 중간 | 높음 | OPA와 Agent 권한 분리, Hard Block 우선 | MITIGATED |

허용 상태는 `OPEN`, `MITIGATED`, `ACCEPTED`, `CLOSED`다. 상태 변경에는 날짜, 검증 근거 또는 관련 ADR을 Notes로 추가한다. `MITIGATED`는 위험이 사라졌다는 뜻이 아니며 Release Readiness에서 잔여 위험을 다시 검토한다.
