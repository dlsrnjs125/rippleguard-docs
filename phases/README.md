# Phase Documentation

이 디렉터리는 Phase 0부터 최종 통합까지 목표, 범위, 참여 Repository, 통합 지점, 진입·종료 조건을 중앙에서 추적한다. 서비스 내부 구현 계획은 복사하지 않고 해당 Repository 문서와 검증 Commit을 연결한다.

## Roadmap

| Phase | 계획 |
| --- | --- |
| 0 | [Foundation & Architecture Baseline](phase-00-foundation/README.md) |
| 1 | [Core MSA Foundation](phase-01-core-msa/README.md) |
| 2 | [Loan Decision Agent](phase-02-loan-decision/README.md) |
| 3 | [RippleGuard Consequence Agent](phase-03-consequence-agent/README.md) |
| 4 | [Evidence & Legacy Control](phase-04-evidence-legacy-control/README.md) |
| 5 | [Assurance Policy & Dynamic Routing](phase-05-assurance-policy/README.md) |
| 6 | [Governance Console](phase-06-governance-console/README.md) |
| 7 | [Audit, Trace & Replay](phase-07-audit-replay/README.md) |
| 8 | [Reliability & Degraded Mode](phase-08-reliability/README.md) |
| 9 | [Evaluation](phase-09-evaluation/README.md) |
| 10 | [Final Integration & Closure](phase-10-final-integration/README.md) |

## 계획과 실행 문서

개발 전 Phase에는 계획용 `README.md`와 `plan.md`만 둔다. Phase가 시작될 때 다음 실행 문서를 생성한다.

```text
progress.md
cross-repo-baseline.md
verification.md
troubleshooting/
evidence/
```

## 상태 모델

| 상태 | 의미 |
| --- | --- |
| `PLANNED` | 목표·범위·의존성만 정의됨 |
| `READY` | 선행 Phase와 필요한 계약이 완료되어 시작 가능 |
| `IN_PROGRESS` | 하나 이상의 관련 Repository에서 실제 작업 진행 중 |
| `BLOCKED` | 계약·선행 서비스·기술 문제로 진행 불가 |
| `IN_REVIEW` | 구현이 끝나 Cross-Repo 통합·검증 중 |
| `VERIFIED` | 완료 기준과 Cross-Repo Baseline 확정 |
| `SUPERSEDED` | 후속 설계나 Phase에 의해 대체됨 |

## Phase 시작

1. [의존성 Map](../tracking/phase-dependency-map.md)과 선행 Phase Baseline을 확인한다.
2. `phase-status.md`를 `READY → IN_PROGRESS`로 변경한다.
3. 실행 문서와 디렉터리를 Template으로 생성한다.
4. 참여 Repository별 Phase Branch와 선행 Contract Version을 기록한다.

## 개발 중 중앙 갱신 기준

서비스 책임, 계약 버전, Phase 범위, 주요 Blocker, 아키텍처 결정, 통합 기준 또는 다른 Repository에 영향을 주는 Troubleshooting이 바뀌면 중앙 문서를 갱신한다. 개별 클래스·함수 구현 변경은 해당 서비스 Repository에만 기록한다.

## Phase 검토와 종료

관련 PR과 독립 빌드가 완료되면 계약, Docker 통합, 장애·예외 시나리오를 검증하고 Evidence와 Baseline 후보를 기록한 뒤 `IN_REVIEW`로 전환한다. 종료 시 병합 또는 검증 Commit, 불변 Image, Contract Version, DB Migration, Verification을 모두 고정해 `VERIFIED`로 변경하고 다음 Phase를 `READY`로 올린다.

문제의 상세 원인과 해결은 소유 Repository에 기록한다. Cross-Repo 문제만 중앙 Phase Troubleshooting에 요약하고 [전체 Index](../tracking/troubleshooting-index.md)에 연결한다.
