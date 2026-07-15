# Phase 9 — Evaluation

- Phase: 9
- 상태: `PLANNED`
- 선행 Phase: Phase 8
- 후속 Phase: Phase 10

## 목표와 필요성

RippleGuard Multi-Agent와 Assurance Gate가 단일 모델·단일 Agent보다 2차 위험을 더 잘 통제하는지 재현 가능한 Golden Case로 평가한다.

## 범위와 경계

- 포함: `Loan Model Only`, `Loan Agent Only`, `Multi-Agent + Assurance Gate` 비교, Golden Case 20~30건, 품질·시간·비용·사람 검토율
- 제외: 실제 고객 성능, 규제 적합성 인증, 통계적으로 일반화된 금융기관 결론
- 변경 금지: 분모 0을 성공으로 처리하거나 합성 데이터 결과를 실제 금융 성능으로 과장하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-agent-runtime` | 비교군 실행과 Agent Metrics |
| `rippleguard-governance-service` | Assurance·Routing 결과 |
| `rippleguard-audit-replay-service` | Golden Replay와 Trace Completeness |
| `rippleguard-infra` | 고정 실행 환경 |
| `rippleguard-docs` | 평가 정의·결과·한계 보고 |

## 선행 조건과 산출물

Reliability 검증된 Baseline과 라벨된 Golden Case가 필요하다. 산출물은 Dataset Version, 실행 Manifest, 지표표, Replay Evidence와 평가 보고서다.

## 통합 지점과 실패 경로

모든 비교군은 같은 Snapshot·Case·환경과 고정 Version을 사용한다. 실행 실패와 누락 Trace를 제외하지 않고 별도 실패 지표로 기록한다.

## 완료 기준

- 위험 유형별 Detection·Escape·Containment·Trace 지표 산출
- 처리시간, Agent 호출 비용, 사람 검토율 비교
- 모든 결과의 Run·Version·Case 재현 가능
- 한계·실패·N/A와 Cross-Repo Baseline 기록

## 다음 Phase 인계 조건

최종 문서가 인용할 검증된 평가 보고서, 한계와 재현 명령이 준비되어야 한다.
