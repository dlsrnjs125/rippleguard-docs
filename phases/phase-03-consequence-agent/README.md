# Phase 3 — RippleGuard Consequence Agent

- Phase: 3
- 상태: `PLANNED`
- 선행 Phase: Phase 2
- 후속 Phase: Phase 4, Phase 5

## 목표와 필요성

Loan Decision이 데이터 목적·의미·판단 범위를 바꾸며 만드는 2차 위험을 탐지해 Consequence Envelope로 반환한다.

## 범위와 경계

- 포함: Cross-Purpose Data Reuse, Scope Escalation, Semantic Transformation, 만료·미확정 추론 사용, FDS 대표 Case
- 제외: Evidence 문서 검증, OPA 최종 실행 경로, 법률 자문
- 변경 금지: RippleGuard Agent는 Loan 결과·Assurance를 확정하거나 Evidence Agent 결론을 입력으로 사용하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | Consequence Envelope와 Event Schema |
| `rippleguard-agent-runtime` | RippleGuard Agent Graph·Tool·검증 |
| `rippleguard-governance-service` | 실행 계획, 결과 Validator와 Case 연결 |
| `rippleguard-infra` | FDS Simulator와 통합 실행 |

## 선행 조건과 산출물

검증된 Decision Envelope, FDS 목적 Metadata와 Golden 위험 Case 초안이 필요하다. 산출물은 Consequence Envelope, Agent Output 검증, 위험 유형별 Test와 Image다.

## 통합 지점과 실패 경로

Governance가 Decision Envelope와 허용 Snapshot으로 실행을 지시하고 Agent Runtime이 결과를 반환한다. 목적 Metadata 누락, 만료, Schema 실패 시 안전하다고 추정하지 않고 검증 필요 또는 차단 후보로 기록한다.

## 완료 기준

- FDS 정보 오용 대표 Case를 위험 유형과 Evidence Reference로 탐지
- 허용 사용 Negative Case의 과탐 결과 기록
- Agent Timeout·Schema·Tool 실패 경로 검증
- 독립 빌드·Docker 통합·Baseline 고정

## 다음 Phase 인계 조건

Evidence Agent와 독립 비교 가능한 Versioned Consequence Envelope와 위험 라벨 Case가 준비되어야 한다.
