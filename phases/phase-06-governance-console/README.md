# Phase 6 — Governance Console

- Phase: 6
- 상태: `PLANNED`
- 선행 Phase: Phase 5
- 후속 Phase: Phase 7

## 목표와 필요성

AI Governance Operator가 Agent 자동화 범위를 통제하고 Loan Reviewer가 확정되지 않은 사실만 확인하는 역할 분리 화면을 구현한다.

## 범위와 경계

- 포함: Agent Registry, 상태 전이, Manual Safe Mode, Segment Isolation, Reviewer Task, Run Trace 요약, RBAC
- 제외: Prompt·Policy 직접 편집, Auditor의 변경 권한, Loan Reviewer의 Agent 설정
- 변경 금지: Operator는 대출을 승인하지 않고 Reviewer는 정책 위반을 Override하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-web` | 역할별 Console·Task UI |
| `rippleguard-governance-service` | Agent 상태·Task·Safe Mode API |
| `rippleguard-audit-replay-service` | 조회용 Trace·변경 이력 API |
| `rippleguard-contracts` | 운영·조회 API 계약 |

## 선행 조건과 산출물

Phase 5의 Agent 상태·정책·Task API가 필요하다. Web 기본 화면은 Phase 5 후반에 병렬 착수할 수 있으나 상태 변경 검증은 Phase 5 완료 후 수행한다. 산출물은 Web Image, API, RBAC Test와 운영 Audit Event다.

## 통합 지점과 실패 경로

Web은 공개 REST API만 호출한다. 긴급 중단은 1인 실행 가능하고 활성화는 Shadow Replay와 이중 확인을 요구한다. API·권한 오류 시 상태를 낙관적으로 표시하거나 자동화 범위를 확대하지 않는다.

## 완료 기준

- Operator·Reviewer·Auditor 권한 분리 Test 통과
- `DISABLED → ACTIVE` 직접 전환 금지와 사유 기록 검증
- Safe Mode·Segment Isolation 안전 동작 검증
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

모든 운영 동작과 Case 조회를 Correlation·Version Metadata로 Audit·Replay에 연결할 수 있어야 한다.
