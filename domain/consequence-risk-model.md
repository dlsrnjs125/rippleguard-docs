# Consequence Risk Model

## 목적

AI가 만든 2차 위험을 다차원으로 측정해 어떤 자동화 범위가 안전한지 결정한다. 실행 경로는 단일 평균 점수가 아니라 금지 조건, 증거·통제 충족 여부, 시스템 건전성을 함께 평가한다.

## 평가 차원

| 차원 | 확인 질문 | 예시 값 |
| --- | --- | --- |
| Purpose Compatibility | 원 데이터의 `permittedUses`가 현재 대출 판단 목적을 허용하는가 | `ALLOWED`, `CONDITIONAL`, `PROHIBITED`, `UNKNOWN` |
| Scope Change | 거래·문서 단위 신호가 신청자 전체 위험으로 확대됐는가 | `NONE`, `BOUNDED`, `ESCALATED`, `UNKNOWN` |
| Inference Status | 관찰·추정·확정 상태가 판단 과정에서 사실화됐는가 | `OBSERVED`, `SUSPECTED`, `CONFIRMED`, `MISREPRESENTED` |
| Evidence Completeness | 중요 주장에 필요한 출처와 유효한 증빙이 있는가 | `COMPLETE`, `PARTIAL`, `MISSING` |
| Legacy Control Coverage | 기존 필수 확인·승인·증빙 절차가 충족됐는가 | `COMPLETE`, `GAP`, `VIOLATED` |
| Downstream Impact | 결과가 조회, 조건 변경, 거절 등 어디까지 영향을 주는가 | `INFORMATIONAL`, `REVIEW`, `TERMS`, `REJECTION` |
| Affected System Count | 파생 신호가 몇 시스템에 전달·사용되는가 | 0 이상의 정수와 system references |
| Reversibility | 결과를 취소하고 영향을 회복할 수 있는가 | `REVERSIBLE`, `PARTIAL`, `IRREVERSIBLE` |
| Model Uncertainty | 모델·Agent 불확실성이 허용 범위 안인가 | calibrated band와 version |
| System Health | Agent, 계약, Tool, 정책, Event 처리 상태가 정상인가 | `HEALTHY`, `DEGRADED`, `UNAVAILABLE` |

각 평가는 원본 목적·범위, Decision Envelope, 독립 Agent 결과와 Evidence Reference를 보존한다. 위험 차원과 enum의 실행 가능한 Schema는 `rippleguard-contracts`에서 확정한다.

## 실행 제약

| 조건 | 분류 | 기본 경로 |
| --- | --- | --- |
| `prohibitedUses`에 포함된 데이터가 판단 근거에 사용됨 | Hard Block | 결정 중단, 영향 전파 차단, 감사 기록 |
| `SUSPECTED` FDS 정보가 확정 사실 또는 고객 신용위험으로 표현됨 | Hard Block | 결정 중단, 정정 또는 재계산 요구 |
| 필수 Legacy Control이 명시적으로 위반됨 | Hard Block | 자동화 중단, 정책 위반 기록 |
| 문서 일부 불일치 또는 필수 Evidence 일부 누락 | Additional Verification | 한정된 사실 확인 후 전체 재평가 |
| 모델 불확실성이 중간 band이거나 calibration이 충분하지 않음 | Additional Verification | Shadow 또는 사람 확인 후 재평가 |
| 계약·정책·필수 Agent Health가 `UNAVAILABLE` | Additional Verification 또는 Manual Safe Mode | 자동화 범위 축소 |

Hard Block은 평균 점수나 사람의 판단으로 상쇄할 수 없다. 선택적 Risk Score가 필요하면 대시보드 정렬과 검토 우선순위에만 사용하며 자동 승인·거절·차단 조건으로 사용하지 않는다.

## 핵심 평가 지표

| 지표 | 정의 |
| --- | --- |
| Consequence Risk Detection Rate | 라벨된 2차 위험 Case 중 시스템이 해당 위험을 탐지한 비율 |
| Consequence Escape Rate | 실제 2차 위험 Case 중 차단·검증 없이 안전 자동화 경로로 빠져나간 비율 |
| Cross-Purpose Reuse Detection Rate | 라벨된 Cross-Purpose Data Reuse Case 중 탐지 비율 |
| Scope Escalation Detection Rate | 라벨된 Scope Escalation Case 중 탐지 비율 |
| Legacy Control Loss Detection Rate | 라벨된 Legacy Control Loss Case 중 탐지 비율 |
| Propagation Containment Rate | 위험 신호가 허용 범위를 넘기 전 차단된 전파 시도 비율 |
| Decision Trace Completeness | 필수 입력·버전·Evidence·Agent Run·정책·상태 Event를 모두 연결한 결정 비율 |

분모가 0인 지표는 100%로 처리하지 않고 `N/A`로 보고한다. 목표값과 허용 임계치는 Golden Case가 준비되는 Phase에서 데이터 근거와 함께 확정한다.
