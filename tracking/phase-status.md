# Phase Status

| Phase | 이름 | 상태 | 선행 Phase | 핵심 종료 조건 |
| --- | --- | --- | --- | --- |
| 0 | [Foundation & Architecture Baseline](../phases/phase-00-foundation/README.md) | VERIFIED | 없음 | 서비스·계약 기준과 전체 Roadmap 검증 |
| 1 | [Core MSA Foundation](../phases/phase-01-core-msa/README.md) | VERIFIED | 0 | AI 없는 최소 수직 흐름, 서비스별 DB와 최소 Timeline 검증 |
| 2 | [Loan Decision Agent](../phases/phase-02-loan-decision/README.md) | BLOCKED | 1 | Versioned Snapshot에서 재현 가능한 Loan Proposal |
| 3 | [RippleGuard Consequence Agent](../phases/phase-03-consequence-agent/README.md) | PLANNED | 2 | 핵심 2차 위험을 Consequence Envelope로 탐지 |
| 4 | [Evidence & Legacy Control](../phases/phase-04-evidence-legacy-control/README.md) | PLANNED | 2, 3 | 증빙·통제 누락을 구조화된 Finding으로 생성 |
| 5 | [Assurance Policy & Dynamic Routing](../phases/phase-05-assurance-policy/README.md) | PLANNED | 3, 4 | OPA 기반 자동화·검증·차단 경로 검증 |
| 6 | [Governance Console](../phases/phase-06-governance-console/README.md) | PLANNED | 5 | 역할이 분리된 Agent·검토 운영 통제와 Static Architecture Graph |
| 7 | [Audit, Trace & Replay](../phases/phase-07-audit-replay/README.md) | PLANNED | 5, 6 | Hash Chain, 상세 Trace, Replay, Version Diff와 Execution Graph 검증 |
| 8 | [Reliability & Degraded Mode](../phases/phase-08-reliability/README.md) | PLANNED | 7 | 장애 시 중복 결정 방지와 자동화 축소 |
| 9 | [Evaluation](../phases/phase-09-evaluation/README.md) | PLANNED | 8 | Golden Case 비교 평가, Graph 사용성·정확성 평가와 한계 보고 |
| 10 | [Final Integration & Closure](../phases/phase-10-final-integration/README.md) | PLANNED | 9 | 재현 가능한 최종 Baseline과 문서 확정 |

허용 상태는 `PLANNED`, `READY`, `IN_PROGRESS`, `BLOCKED`, `IN_REVIEW`, `VERIFIED`, `SUPERSEDED`다. 상태 변경은 선행 조건, Verification과 Cross-Repo Baseline에 근거하며 변경 이유는 [Roadmap Change Log](roadmap-change-log.md)에 남긴다.

Phase 0은 Contracts, Infra, Docs finalization 결과가 main에 병합됐고 Published Baseline이 고정되어 `VERIFIED`다. Phase 1은 서비스 OCI provenance 보완 PR과 Infra runtime verification PR이 main에 병합됐고 필수 runtime checks가 PASS로 기록되어 `VERIFIED`다. Phase 2는 구현 PR들이 main에 병합됐지만 최종 Cross-Repository Gate에서 full E2E, image digest provenance, Loan-to-Agent feature snapshot path가 충족되지 않아 `BLOCKED`다.

## Phase 1 Exit Evidence

1. Loan service image provenance
   - Owner: `rippleguard-loan-service`
   - Result: PASS with merge commit `e403c0a60ccb1cebf03380832d047f3fc01019e0`

2. Governance service image provenance
   - Owner: `rippleguard-governance-service`
   - Result: PASS with merge commit `4e06e672affddc02d7e6662f3022d00de86bb3b9`

3. Audit service image provenance
   - Owner: `rippleguard-audit-replay-service`
   - Result: PASS with merge commit `83ca52edda2f608f90d10694428dff6dffee8a23`

4. Infra manifest update and runtime verification
   - Owner: `rippleguard-infra`
   - Result: PASS with merge commit `4499fa6a321d5bd1305ec6f07910fbc9c3096db4`
   - Verified: image verification, Phase 1 check, E2E, duplicate, recovery, outbox recovery and timeline checks

5. Final published baseline and status transition
   - Owner: `rippleguard-docs`
   - Result: PR #7 merged to main as `86b6c8c`; Phase 1 `VERIFIED` and Phase 2 `READY` are published project status

Note: the Phase 2 `READY` handoff above was superseded by the 2026-07-22 Phase 2 final cross-repository review. Current Phase 2 status is `BLOCKED`.
