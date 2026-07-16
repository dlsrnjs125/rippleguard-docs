# Phase 6 — Governance Console

- Phase: 6
- 상태: `PLANNED`
- 선행 Phase: Phase 5
- 후속 Phase: Phase 7

## 목표와 필요성

AI Governance Operator가 Agent 자동화 범위를 통제하고 Loan Reviewer가 확정되지 않은 사실만 확인하는 역할 분리 화면을 구현한다. 또한 Versioned Topology Manifest 기반 Static Architecture Graph로 핵심 서비스·Agent·Tool·Policy·Event 관계를 설명한다.

## 범위와 경계

- 포함: Agent Registry, 상태 전이, Manual Safe Mode, Segment Isolation, Reviewer Task, Phase 1~2 최소 Timeline 기반 Run Trace 요약, RBAC, Static Architecture Graph, Graph Search·Filter·Legend·Detail Panel
- 제외: Prompt·Policy 직접 편집, Auditor의 변경 권한, Loan Reviewer의 Agent 설정, 실시간 EvaluationRun Graph, Golden Replay Graph, 학습 데이터 Provenance, 전체 로그 Graph
- 변경 금지: Operator는 대출을 승인하지 않고 Reviewer는 정책 위반을 Override하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-web` | 역할별 Console·Task UI |
| `rippleguard-governance-service` | Agent 상태·Task·Safe Mode API |
| `rippleguard-audit-replay-service` | Phase 1~2에서 구축한 최소 Timeline·변경 이력 조회 API |
| `rippleguard-contracts` | 운영·조회 API 계약 |
| `rippleguard-infra` | Topology Manifest가 대조할 서비스·Topic·Policy 배치 기준 |

## 선행 조건과 산출물

Phase 5의 Agent 상태·정책·Task API와 Phase 1~2의 최소 Trace API가 필요하다. Web 기본 화면은 Phase 5 후반에 병렬 착수할 수 있으나 상태 변경 검증은 Phase 5 완료 후 수행한다. Static Architecture Graph는 `rippleguard-infra`가 소유·배포하는 Versioned Topology Manifest를 사용하며 실시간 서비스 디스커버리나 운영 DB 직접 조회에 의존하지 않는다. Manifest Schema와 Graph Core DTO는 `rippleguard-contracts`에서 먼저 확정한다. Phase 6은 Golden Replay·Version Diff·상세 Trace UI와 Execution Graph를 구현하지 않는다.

## 통합 지점과 실패 경로

Web은 공개 REST API만 호출한다. 긴급 중단은 1인 실행 가능하고 활성화는 Shadow Replay와 이중 확인을 요구한다. API·권한 오류 시 상태를 낙관적으로 표시하거나 자동화 범위를 확대하지 않는다.

## 완료 기준

- Operator·Reviewer·Auditor 권한 분리 Test 통과
- `DISABLED → ACTIVE` 직접 전환 금지와 사유 기록 검증
- Safe Mode·Segment Isolation 안전 동작 검증
- 핵심 Service·Agent·Tool·Policy 관계 표시
- Kafka Publish·Subscribe 방향 표시
- Node 선택과 상세 패널 연결
- Role별 민감정보 비노출
- Graph와 현재 Architecture Baseline 일치
- 기본 Node·Link Budget 내 가독성 확인
- Graph 데이터가 실제 서비스 DB 직접 조회에 의존하지 않음
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

모든 운영 동작과 Case 조회를 Correlation·Version Metadata로 Audit·Replay에 연결할 수 있어야 한다.
