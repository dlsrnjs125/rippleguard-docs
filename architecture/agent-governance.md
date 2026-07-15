# Agent Governance

- Agent는 분석과 제안을 생성할 뿐 승인, 거절, 지급 등 최종 금융 행동을 실행할 수 없다.
- Agent는 등록되고 목적·입력 범위가 승인된 Tool만 사용한다.
- 모든 Agent Output은 `rippleguard-contracts`의 JSON Schema로 검증한다.
- 중요 주장과 결과에는 추적 가능한 Evidence Reference가 필요하다.
- 운영 상태는 `ACTIVE`, `SHADOW_ONLY`, `DISABLED` 중 하나이며 Governance Service가 관리한다.
- Governance Service만 Evidence와 정책 결과를 종합해 Assurance 상태와 실행 경로를 확정한다.
- Loan Service만 최종 대출 상태를 변경한다.
- Agent 오류, 계약 불일치, 증거 부족 또는 정책 엔진 장애 시 자동화 범위를 줄이고 추가 검증이나 Manual Safe Mode로 전환한다.

## 독립 검증과 실행 계획

Governance Service가 Preflight 결과와 Case 조건을 바탕으로 필요한 전문 Agent와 병렬·순차 관계를 포함한 실행 계획을 결정한다. Agent Runtime은 전달받은 계획과 허용된 입력 범위대로 실행하며 정책적으로 어떤 Agent가 필요한지 임의 결정하지 않는다.

RippleGuard Agent와 Evidence & Control Agent는 동일한 Decision Envelope를 기준으로 독립 실행한다. 서로의 최종 결론은 입력으로 사용하지 않고 Governance Service가 Schema와 provenance를 검증한 뒤 비교한다.

## 결정론적 검사와 Agent 검사 경계

| 검사 | 담당 |
| --- | --- |
| 데이터 목적 허용 목록과 `prohibitedUses` | Deterministic Preflight |
| `validUntil`, `inferenceStatus`, 필수 Metadata | Deterministic Preflight |
| 알려진 `subjectType`과 허용 Scope 전이 | Deterministic Preflight |
| 자연어 설명에서의 의미 변질·미확정 사실화 | RippleGuard Agent |
| 복수 후속 행동 결합의 Downstream Impact | RippleGuard Agent |
| 문서와 DB·Snapshot 값 대조 | Evidence & Control Agent + Deterministic Validator |
| 필수 통제 실행·Coverage | Control Registry + Policy |
| 최종 실행 경로 | OPA + Governance Service |

명시적인 enum·시간·allowlist 비교를 LLM에 위임하지 않는다. RippleGuard Agent는 여러 Context를 거치며 생긴 미묘한 의미 변화와 문서화되지 않은 업무 맥락을 분석하고, 그 결과도 결정론적 Schema·정책 검증을 거친다.

## 조건부 실행 Trigger

| 조건 | RippleGuard Agent | Evidence & Control Agent |
| --- | --- | --- |
| 외부 FDS 신호 없음·문서 충돌 없음 | 생략 가능 | 생략 가능 |
| FDS 정보가 심사 Feature에 포함 | 실행 | Evidence 요구가 있으면 실행 |
| 거래 단위 신호가 신청자 Scope로 변경 | 실행 | 생략 가능 |
| DB 소득과 정산서 충돌 | 관련 의미 변질이 있으면 실행 | 실행 |
| Legacy Control이 문서 확인 요구 | 생략 가능 | 실행 |
| 고영향 Case 표본 감사 | 실행 | 실행 |
| Agent·Policy 신규 Version Shadow | 실행 | 실행 |

Governance Service가 Versioned Trigger Rule로 실행 계획을 만들고 Trigger 근거와 생략 사유를 Evaluation Run에 기록한다. Agent Runtime은 계획을 임의 확대·축소하지 않는다.

Trigger 구현 오류로 위험 Case가 우회되는 것을 탐지하기 위해 `SAFE_AUTOMATION` 후보 중 일부를 무작위와 위험 기반으로 **Sample Audit**에 배정해 두 전문 Agent를 모두 Shadow 실행한다. 표본 비율·Seed·선정 규칙을 Version으로 고정하고, 발견된 Escape는 정책·Trigger 회귀 Case와 Risk 지표에 반영한다. Sample Audit 결과는 해당 운영 결정을 소급 변경하지 않으며 안전 문제 발견 시 별도 차단·재평가 절차를 시작한다.

Prompt, 모델, Tool 또는 정책 변경은 Golden Case Replay와 Shadow Mode 비교를 통과한 뒤 활성화한다. 정책 엔진이나 Governance Service를 우회하는 Tool과 직접 상태 변경 경로는 허용하지 않는다.
