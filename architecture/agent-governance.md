# Agent Governance

- Agent는 분석과 제안을 생성할 뿐 승인, 거절, 지급 등 최종 금융 행동을 실행할 수 없다.
- Agent는 등록되고 목적·입력 범위가 승인된 Tool만 사용한다.
- 모든 Agent Output은 `rippleguard-contracts`의 JSON Schema로 검증한다.
- 중요 주장과 결과에는 추적 가능한 Evidence Reference가 필요하다.
- 운영 상태는 `ACTIVE`, `SHADOW_ONLY`, `DISABLED` 중 하나이며 Governance Service가 관리한다.
- Governance Service만 Evidence와 정책 결과를 종합해 Assurance 상태와 실행 경로를 확정한다.
- Loan Service만 최종 대출 상태를 변경한다.
- Agent 오류, 계약 불일치, 증거 부족 또는 정책 엔진 장애 시 자동화 범위를 줄이고 추가 검증이나 Manual Safe Mode로 전환한다.

Prompt, 모델, Tool 또는 정책 변경은 Golden Case Replay와 Shadow Mode 비교를 통과한 뒤 활성화한다. 정책 엔진이나 Governance Service를 우회하는 Tool과 직접 상태 변경 경로는 허용하지 않는다.
