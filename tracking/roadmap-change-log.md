# Roadmap Change Log

Phase 추가·삭제·분리, 서비스·Repository 경계, 핵심 Agent 역할, 기술 스택, MVP 범위 또는 완료 기준이 바뀔 때 기록한다. 단순 문구·오탈자 수정은 기록하지 않는다.

| 날짜 | 변경 전 | 변경 후 | 원인 | 영향 Phase | 관련 ADR |
| --- | --- | --- | --- | --- | --- |
| 2026-07-15 | Rights Advisor Agent | Evidence & Control Agent | 법률 자문을 제외하고 결정 전 Evidence·Legacy Control 검증에 집중 | 3, 4 | [ADR-005](../adr/ADR-005-mvp-agent-role-scope.md) |
| 2026-07-15 | Phase 0 중심 추적 | Phase 0~10 전체 Delivery Roadmap | 프로젝트 종료까지 의존성·검증 Baseline을 중앙 관리 | 전체 | N/A |
| 2026-07-16 | Graph Visualization 범위 미정 | Phase 6 Static Architecture Graph, Phase 7 EvaluationRun Execution Graph, Phase 9 Graph 사용성·정확성 평가 추가. Data Provenance Graph는 MVP 제외 | Multi-Agent 구조와 실행 인과관계를 설명·추적하되 Graph를 Source of Truth로 만들지 않기 위함 | 6, 7, 9, 10 | [ADR-006](../adr/ADR-006-agent-graph-visualization-scope.md) |
| 2026-07-16 | Phase 0 `IN_REVIEW`, Phase 1 `PLANNED` | Phase 0 `VERIFIED`, Phase 1 `IN_PROGRESS` | Phase 0 Contracts·Infra·Docs published baseline을 main merge commit으로 고정하고 Phase 1 Core MSA tracking 시작 | 0, 1 | N/A |
| 2026-07-20 | Phase 1 final baseline expected after implementation PRs | Phase 1 verification deferred: Phase 1 `IN_REVIEW`, Phase 2 `PLANNED` 유지 | Service image OCI provenance is missing, Infra runtime tests are not complete and Integration Matrix required capability correction | 1, 2 | N/A |

## 2026-07-20 — Phase 1 Verification Deferred

Decision:

- Phase 1 was not promoted to `VERIFIED`.
- Phase 2 was not promoted to `READY`.

Reason:

- Service image OCI provenance labels are missing.
- Infra runtime tests are not completed.
- Integration Matrix required capability correction against the Infra manifest.

계획이 대체되더라도 이전 문서와 ADR을 삭제하지 않는다. 변경된 Phase는 필요하면 `SUPERSEDED`로 표시하고 새 경로와 인계 조건을 연결한다.
