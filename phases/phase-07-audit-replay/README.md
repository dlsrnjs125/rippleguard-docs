# Phase 7 — Audit, Trace & Replay

- Phase: 7
- 상태: `PLANNED`
- 선행 Phase: Phase 5, Phase 6
- 후속 Phase: Phase 8

## 목표와 필요성

FDS 입력부터 최종 Loan 상태까지 판단과 전파 경로를 추적하고 Version 변경 영향을 운영 결정과 격리된 Sandbox에서 재현한다.

## 범위와 경계

- 포함: Case Timeline, Correlation·Causation ID, Agent Run, Version Metadata, Tamper-Evident Log, Single·Golden Case Replay, Version Diff
- 제외: Replay 결과의 운영 상태 직접 반영, 물리적으로 변경 불가능한 WORM 보장
- 변경 금지: Audit Service는 운영 DB와 원 Event를 수정하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-audit-replay-service` | Trace 저장·조회·Replay·Diff |
| `rippleguard-web` | Timeline·Replay·Diff UI |
| `rippleguard-infra` | Audit DB·MinIO·관측 환경 |
| `rippleguard-contracts` | Trace·Replay 계약 |

## 선행 조건과 산출물

전 서비스 Correlation·Version Metadata와 운영 Event가 필요하다. 산출물은 Append-only Schema, Hash Chain, Replay API·Runner, UI, Verification 결과다.

## 통합 지점과 실패 경로

Audit Service가 Kafka Event를 소비하고 Object Reference로 Evidence를 연결한다. 누락·순서 역전·Hash 불일치는 원본을 수정하지 않고 Gap 또는 Tamper 의심으로 표시하며 Replay 실패를 운영 결정과 격리한다.

## 완료 기준

- 전체 Case Timeline 조회와 Single Case Replay 성공
- Golden Case Replay와 Version Diff 재현
- Hash Chain 변경 탐지와 Trace Gap 표시 검증
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

장애 Drill에서 결과를 판정할 Trace, 중복·순서·Replay 관측 기준이 준비되어야 한다.
