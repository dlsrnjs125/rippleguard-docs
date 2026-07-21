# Phase 8 — Reliability & Degraded Mode

- Phase: 8
- 상태: `PLANNED`
- 선행 Phase: Phase 7
- 후속 Phase: Phase 9

## 목표와 필요성

Agent, Kafka, OPA, DB 장애와 Event 중복·순서 역전 상황에서도 위험한 금융 결정과 중복 최종화가 실행되지 않게 한다.

## 범위와 경계

- 포함: Retry, Timeout, DLT, Circuit Breaker, 중복·순서 역전, Fail-Closed, Manual Safe Mode, Segment Isolation, 장애 Runbook, Ollama Down, Model Load Failure, artifact/provider/Modelfile Digest Mismatch, Inference Timeout, Context Window Overflow, Malformed Structured Output, Memory Pressure, Agent Runtime과 Model Runtime Network Failure
- 제외: 무중단 운영 SLA 보장, Kubernetes 복구, 외부 금융기관 DR
- 변경 금지: 장애 시 검증을 생략하거나 마지막 성공 결과를 새 결정으로 재사용하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-infra` | 장애 주입·관측·통합 환경 |
| 모든 서비스 Repository | Timeout·Retry·멱등·Degraded Mode |
| `rippleguard-contracts` | Error·DLT·Health 계약 |
| `rippleguard-docs` | Drill 기준·Runbook·결과 추적 |

## 선행 조건과 산출물

전체 수직 흐름과 Trace가 필요하다. 산출물은 장애 시나리오, Runbook, Health Signal, DLT·복구 Evidence, Image와 Verification이다.

## 통합 지점과 실패 경로

Kafka, DB, OPA, Agent 장애를 서비스 경계별로 주입한다. 복구 전 자동 결정은 보류·차단하고, 재처리는 멱등 Key와 Causation Chain을 유지한다. Sample Audit Escape가 확인되면 위험 유형을 등록하고 관련 Segment 자동화를 축소한 뒤 Phase 7 검색 결과를 사용해 영향 가능 Run마다 새 Evaluation Run을 생성한다. 이전·새 결과를 비교하고 필요한 Case만 재검토한다.

Local LLM 장애는 `Agent Failure -> Evaluation Failed -> Governance VERIFICATION_REQUIRED -> 자동화 축소` 경로로 처리한다. Mock 자동 대체, 이전 결과 재사용, 자동 승인, Validation 비활성화는 금지한다.

## 완료 기준

- 중복·순서 역전·재시도에서 최종 결정 1회 보장
- 필수 Agent·OPA·DB 장애 시 안전한 자동화 축소
- DLT 복구와 Manual Safe Mode Drill 증빙
- `Escape 발견 → Segment 축소 → 영향 Run 검색 → 신규 Run → 결과 비교·재검토` Drill 통과
- Runbook, Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

Golden Case 평가가 인프라 불안정과 제품 품질을 구분할 수 있는 안정적 실행·관측 환경이 준비되어야 한다.
