# Phase 7 — Audit, Trace & Replay

- Phase: 7
- 상태: `PLANNED`
- 선행 Phase: Phase 5, Phase 6
- 후속 Phase: Phase 8

## 목표와 필요성

FDS 입력부터 최종 Loan 상태까지 판단과 전파 경로를 추적하고 Version 변경 영향을 운영 결정과 격리된 Sandbox에서 재현한다. Case·EvaluationRun Execution Graph로 특정 Run의 입력, Agent, Event, 정책, 최종 결과 경로를 인과관계로 탐색한다.

## 범위와 경계

- 포함: Case Timeline, Correlation·Causation ID, Agent Run, Version Metadata, Tamper-Evident Log, Single·Golden Case Replay, Version Diff, Case·EvaluationRun Execution Graph, Event Directional Particle, Run 간 `supersedesRunId` 관계, 시간순 Playback 또는 Step Highlight
- 제외: Replay 결과의 운영 상태 직접 반영, 물리적으로 변경 불가능한 WORM 보장, 학습 데이터 Provenance Graph, 전체 로그 Graph
- 변경 금지: Audit Service는 운영 DB와 원 Event를 수정하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-audit-replay-service` | Trace 저장·조회·Replay·Diff |
| `rippleguard-web` | Timeline·Replay·Diff UI, Execution Graph UI |
| `rippleguard-infra` | Audit DB·MinIO·관측 환경 |
| `rippleguard-contracts` | Trace·Replay 계약 |

## 선행 조건과 산출물

Phase 1~2의 원본 Event·최소 Timeline, 전 서비스 Correlation·Causation·Version Metadata와 Phase 6 운영 Event가 필요하다. Phase 7은 기존 Foundation을 대체하지 않고 Hash Chain, Golden Replay, Version Diff, 영향 범위 Replay, Execution Graph Read Model과 상세 Trace UI로 확장한다.

## 통합 지점과 실패 경로

Audit Service가 Kafka Event를 소비하고 Object Reference로 Evidence를 연결한다. Execution Graph는 Audit Read Model에서 제공하며 Web이 Kafka 또는 서비스 DB를 직접 읽지 않는다. 누락·순서 역전·Hash 불일치는 원본을 수정하지 않고 Gap 또는 Tamper 의심으로 표시하며 Replay 실패를 운영 결정과 격리한다.

## 완료 기준

- 전체 Case Timeline 조회와 Single Case Replay 성공
- Golden Case Replay와 Version Diff 재현
- Sample Audit Escape를 위험 유형으로 등록하고 영향 가능 Evaluation Run을 검색할 수 있음
- Escape Event, 영향 Run 목록과 비교 Replay 계약 검증
- Hash Chain 변경 탐지와 Trace Gap 표시 검증
- Case ID로 Execution Graph 조회
- EvaluationRun 단위 필터
- Agent 실행·생략 이유 표시
- Event 인과관계 표현
- Hard Block·Verification·Safe Automation 경로 구분
- 이전 Run과 재산출 Run 연결
- Trace Gap 또는 순서 역전 표시
- Graph와 Timeline 결과의 의미상 일치 검증
- 부분 Graph 응답이 전체 Trace로 오인되지 않음
- 적용 Filter와 누락 Node·Link 개수 표시
- Trace Gap과 Budget·Filter·Retention 생략 구분
- Pagination 또는 Expand 조회로 누락 범위 탐색 가능
- 원문 Prompt·PII·문서 원문 비노출
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

장애 Drill에서 결과를 판정할 Trace, 중복·순서·Replay 관측 기준과 Sample Audit Escape 영향 Run 검색 결과가 준비되어야 한다.
