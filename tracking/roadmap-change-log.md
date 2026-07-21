# Roadmap Change Log

Phase 추가·삭제·분리, 서비스·Repository 경계, 핵심 Agent 역할, 기술 스택, MVP 범위 또는 완료 기준이 바뀔 때 기록한다. 단순 문구·오탈자 수정은 기록하지 않는다.

| 날짜 | 변경 전 | 변경 후 | 원인 | 영향 Phase | 관련 ADR |
| --- | --- | --- | --- | --- | --- |
| 2026-07-15 | Rights Advisor Agent | Evidence & Control Agent | 법률 자문을 제외하고 결정 전 Evidence·Legacy Control 검증에 집중 | 3, 4 | [ADR-005](../adr/ADR-005-mvp-agent-role-scope.md) |
| 2026-07-15 | Phase 0 중심 추적 | Phase 0~10 전체 Delivery Roadmap | 프로젝트 종료까지 의존성·검증 Baseline을 중앙 관리 | 전체 | N/A |
| 2026-07-16 | Graph Visualization 범위 미정 | Phase 6 Static Architecture Graph, Phase 7 EvaluationRun Execution Graph, Phase 9 Graph 사용성·정확성 평가 추가. Data Provenance Graph는 MVP 제외 | Multi-Agent 구조와 실행 인과관계를 설명·추적하되 Graph를 Source of Truth로 만들지 않기 위함 | 6, 7, 9, 10 | [ADR-006](../adr/ADR-006-agent-graph-visualization-scope.md) |
| 2026-07-16 | Phase 0 `IN_REVIEW`, Phase 1 `PLANNED` | Phase 0 `VERIFIED`, Phase 1 `IN_PROGRESS` | Phase 0 Contracts·Infra·Docs published baseline을 main merge commit으로 고정하고 Phase 1 Core MSA tracking 시작 | 0, 1 | N/A |
| 2026-07-20 | Phase 1 final baseline expected after implementation PRs | Phase 1 verification deferred: Phase 1 `IN_REVIEW`, Phase 2 `PLANNED` 유지 | Service image OCI provenance is missing, Infra runtime tests are not complete and Integration Matrix required capability correction | 1, 2 | N/A |
| 2026-07-20 | Phase 1 `IN_REVIEW`, Phase 2 `PLANNED` | Phase 1 `VERIFIED`, Phase 2 `READY` | Service OCI provenance remediation, Governance ordering remediation and Infra runtime reverification all passed | 1, 2 | N/A |
| 2026-07-20 | Local LLM strategy implicit; external API and agent-specific model choices not bounded | Hybrid Multi-Agent strategy accepted: Loan Decision Agent remains tabular ML, RippleGuard·Evidence Agents are Local LLM first with Ollama, external API is optional benchmark, Phase 3 is first Local LLM integration | Architecture boundary, MVP runtime and Phase gates needed a versioned decision | 2, 3, 4, 8, 9, 10 | [ADR-007](../adr/ADR-007-local-llm-first-agent-strategy.md) |

## 2026-07-20 — Phase 1 Verification Deferred

Decision:

- Phase 1 was not promoted to `VERIFIED`.
- Phase 2 was not promoted to `READY`.

Reason:

- Service image OCI provenance labels are missing.
- Infra runtime tests are not completed.
- Integration Matrix required capability correction against the Infra manifest.

## 2026-07-20 — Phase 1 Verified

Decision:

- Phase 1 was promoted to `VERIFIED`.
- Phase 2 was promoted to `READY`.

Reason:

- Loan OCI remediation PR #2 merged at `e403c0a60ccb1cebf03380832d047f3fc01019e0`.
- Governance OCI remediation PR #2 merged at `45790ebd5de1c458f87a38b1a067b46c15a59134`.
- Governance event ordering PR #3 merged at `4e06e672affddc02d7e6662f3022d00de86bb3b9`.
- Audit OCI remediation PR #2 merged at `83ca52edda2f608f90d10694428dff6dffee8a23`.
- Infra runtime verification PR #3 merged at `4499fa6a321d5bd1305ec6f07910fbc9c3096db4`.
- `phase1-check`, `phase1-e2e`, duplicate, recovery, outbox recovery and order checks all passed.

Deferred:

- Registry Digest
- GHCR Publish
- SBOM
- Build Attestation
- SLSA Provenance
- Release Flyway Checksum Pinning

## 2026-07-20 — Local LLM First Agent Strategy

Decision:

- RippleGuard is a Hybrid Multi-Agent Governance Platform.
- Loan Decision Agent keeps a structured ML path with XGBoost or LightGBM and SHAP.
- RippleGuard Consequence Agent and Evidence & Control Agent are Local LLM first.
- MVP Local Runtime uses Ollama.
- External API LLMs move to optional Benchmark and Provider Adapter candidates.
- Agent-specific separate foundation models are excluded from the initial scope.

Phase impact:

- Phase 2 defines tabular model and provider boundaries but does not implement Ollama inference.
- Phase 3 is the first Local LLM integration.
- Phase 4 initially shares the Phase 3 Local LLM Runtime and Model.
- Phase 8 adds Local Model Runtime failure drills.
- Phase 9 compares local multi-agent paths and optional external API benchmarks.

Merge guard:

- Model Manifest Schema must be owned by `rippleguard-contracts`; manifest instances and published baselines must be tracked separately.
- Phase 3 must fix digest semantics before Phase 8 digest mismatch drills.
- Phase 2 does not implement `StructuredLlmPort`; executable LLM provider interfaces move to Phase 3.
- Phase 4 requires a minimum shared-model common-mode regression gate before closure.
- Phase 2 requires a Tabular Model Manifest for XGBoost/LightGBM reproducibility.
- External API providers are not automatic runtime fallbacks; they require explicit policy, data boundary, provider manifest, evaluation baseline and audit trace.
- Phase 3 stability means repeated-run semantic agreement and reference validity metrics, not byte-identical LLM output.
- Partial context results must carry `contextCompleteness`; required evidence omission cannot be marked as normal completion.

계획이 대체되더라도 이전 문서와 ADR을 삭제하지 않는다. 변경된 Phase는 필요하면 `SUPERSEDED`로 표시하고 새 경로와 인계 조건을 연결한다.
