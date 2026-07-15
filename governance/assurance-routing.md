# Assurance Routing

Governance Service는 [Consequence Risk Model](../domain/consequence-risk-model.md)의 다차원 결과와 Assurance Profile을 OPA에 전달해 실행 경로를 결정한다. 단일 평균 Risk Score로 경로를 선택하지 않는다.

## 우선순위

1. 입력 Schema, provenance, System Health를 검증한다.
2. 금지 데이터 사용, 미확정 사실화, 필수 통제 위반 등 Hard Block을 먼저 평가한다.
3. Evidence Completeness와 Legacy Control Coverage의 보완 가능성을 평가한다.
4. Hard Block이 없고 모든 필수 Assurance가 충족된 경우에만 안전 자동화를 허용한다.

## 경로

| 정책 결과 | 조건 | Governance 동작 | Loan Service 영향 |
| --- | --- | --- | --- |
| `HARD_BLOCK` | 금지 목적 사용, 미확정 FDS 사실화, 필수 통제 위반 | Case를 `BLOCKED`로 전이하고 전파 중단·감사 Event 기록 | 최종 결과 명령을 보내지 않거나 명시적 차단 결과 전송 |
| `ADDITIONAL_VERIFICATION` | 증거 일부 누락·불일치, 중간 불확실성, 복구 가능한 Health 저하 | `VERIFICATION_REQUIRED`, 확인할 사실과 허용 Evidence 범위 생성 | `EVIDENCE_REQUIRED` |
| `RECALCULATION` | 정형 사실 수정으로 결과 재평가 가능 | `RECALCULATION_REQUIRED`, 이전 결과를 대체하지 않고 새 Run 생성 | 검토 상태 유지 |
| `MANUAL_SAFE_MODE` | 정책·필수 Agent·계약 검증 불가 또는 운영자 긴급 중단 | 자동화 중단, 승인된 수동 절차로 축소 | 자동 최종화 금지 |
| `SAFE_AUTOMATION` | Hard Block 없음, 필수 Evidence·Control·Health 충족 | `RESOLVED`, 서명된 결정 명령과 Trace 생성 | `DECISION_RECEIVED` 후 최종화 |

사람 확인은 Hard Block을 Override하지 않는다. 확인 가능한 사실을 정형 입력으로 반영한 뒤 Preflight, Agent, Assurance, 정책을 새 Run으로 재실행한다. OPA 정책 입력·결과 버전과 각 분기 근거는 Audit Trace에 연결한다.
