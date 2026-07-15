# Phase 5 — Assurance Policy & Dynamic Routing

- Phase: 5
- 상태: `PLANNED`
- 선행 Phase: Phase 3, Phase 4
- 후속 Phase: Phase 6, Phase 7

## 목표와 필요성

독립 Agent와 결정론적 검증 결과를 OPA 정책으로 연결해 안전 자동화, 추가 검증, 재산출, Hard Block 경로를 확정한다.

## 범위와 경계

- 포함: Purpose·Scope·Control·Action Policy, Assurance 상태, System Health, 제한적 Human Verification, 재산출
- 제외: Agent의 자연어 정책 판단, Hard Block 사람 Override, Console 전체 UI
- 변경 금지: 단일 평균 Risk Score로 실행을 결정하지 않고 Governance Service만 Assurance와 Routing을 확정한다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | Assurance Profile·Policy Input/Result 계약 |
| `rippleguard-governance-service` | 입력 종합, 상태와 Routing 처리 |
| `rippleguard-infra` | OPA Bundle·Version과 통합 환경 |
| `rippleguard-loan-service` | Evidence 요청·결정 Command 처리 |

## 선행 조건과 산출물

Consequence, Evidence Findings, Control Coverage와 Health 입력이 필요하다. 산출물은 OPA Policy Bundle, Assurance Profile, Routing Event/API, 재평가 Trace와 Test 결과다.

## 통합 지점과 실패 경로

Governance가 Versioned Profile을 OPA에 동기 요청하고 결과에 따라 Event를 발행한다. OPA·계약·필수 입력 장애 시 승인으로 우회하지 않고 Manual Safe Mode 또는 검증 경로로 축소한다.

## 완료 기준

- 구조화된 조건으로 자동화·추가 검증·차단 결정
- 금지 사용과 미확정 FDS 사실화 Hard Block 검증
- 사람 확인 후 전체 새 Run 재실행 검증
- Policy Version, Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

Console과 Audit가 소비할 Agent 상태·Policy·Verification Task·Trace API가 준비되어야 한다.
