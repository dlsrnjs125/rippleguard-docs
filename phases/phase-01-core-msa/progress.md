# Phase 1 Progress

Phase 1은 implementation merge 상태와 published verification 상태를 분리해서 추적한다. 구현 PR 병합 후 image provenance와 runtime E2E 재검증이 PASS가 되어 Phase 1은 `VERIFIED`다.

## Contracts

Status: COMPLETE

Evidence:

- `rippleguard-contracts` PR #3 merged at `29f6c348fd93633476438ee36b3f93a3d036e165`.
- GitHub check `validate` succeeded.
- Local `make validate` passed: 30 schemas, 1 OpenAPI document, 50 valid examples, 78 intentional invalid examples, 8 scenarios and 16 fixture-backed compatibility checks.

Remaining:

- None for Phase 1 contract baseline.

Owner:

- `rippleguard-contracts`

## Loan

Status: COMPLETE

Completed:

- Loan Application REST.
- Versioned Snapshot persistence.
- Outbox publication for `loan.application.submitted.v1` and `loan.decision.finalized.v1`.
- Final decision command application.
- Service tests: `./mvnw test` PASS, 24 tests.

Remediation:

- OCI-label follow-up PR #2 merged at `e403c0a60ccb1cebf03380832d047f3fc01019e0`.
- Final image baseline: `rippleguard-loan-service:e403c0a60ccb`.
- OCI labels verified by Infra PR #3.

Owner:

- `rippleguard-loan-service`

Blocks:

- None for Phase 1.

## Governance

Status: COMPLETE

Completed:

- Decision Case and Evaluation Run persistence.
- Deterministic Mock Evaluation.
- Decision Command outbox.
- Service tests: `make test` PASS, 17 tests.

Remediation:

- OCI-label follow-up PR #2 merged at `45790ebd5de1c458f87a38b1a067b46c15a59134`.
- Event ordering follow-up PR #3 merged at `4e06e672affddc02d7e6662f3022d00de86bb3b9`.
- Final image baseline: `rippleguard-governance-service:4e06e672affd`.
- OCI labels and ordering checks verified by Infra PR #3.

Owner:

- `rippleguard-governance-service`

Blocks:

- None for Phase 1.

## Audit

Status: COMPLETE

Completed:

- Core event ingestion.
- Deduplication and malformed event handling.
- Privacy sanitization.
- Minimal Case Timeline API.
- Service tests: `./mvnw test` PASS, 17 tests.

Remediation:

- OCI-label follow-up PR #2 merged at `83ca52edda2f608f90d10694428dff6dffee8a23`.
- Final image baseline: `rippleguard-audit-replay-service:83ca52edda2f`.
- OCI labels verified by Infra PR #3.

Owner:

- `rippleguard-audit-replay-service`

Blocks:

- None for Phase 1.

## Infra Static Integration

Status: COMPLETE

Evidence:

- `rippleguard-infra` PR #2 merged at `ebdd864b279e39701f2bb21750a5080a827943f2`.
- GitHub check `static-validation` succeeded.
- Local `make validate-static` passed.
- Contract topic baseline and manifest source commits were cross-checked.

Remaining:

- None for static validation.

Owner:

- `rippleguard-infra`

## Image Baseline

Status: COMPLETE

Evidence:

- Initial `python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` failed because OCI labels were missing.
- Reverification `make phase1-build-images` PASS.
- Reverification `make phase1-verify-images` PASS.

Root Cause:

- Initial Loan, Governance and Audit service images were built without required OCI source/revision labels.

Resolution:

- Service repository follow-up PRs added labels.
- Infra rebuilt images from clean service checkouts matching manifest source commits.
- Final tags: `e403c0a60ccb`, `4e06e672affd`, `83ca52edda2f`.

Owner:

- Primary: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-audit-replay-service`
- Secondary: `rippleguard-infra`

## Runtime E2E

Status: COMPLETE

Evidence:

- `make phase1-check` PASS.
- `make phase1-e2e` PASS.
- `make phase1-duplicate-check` PASS.
- `make phase1-recovery-check` PASS.
- `make phase1-outbox-recovery-check` PASS.
- `make phase1-order-check` PASS.
- `make phase1-down` PASS.

Remediated commands:

- `make phase1-check`
- `make phase1-e2e`
- `make phase1-duplicate-check`
- `make phase1-recovery-check`
- `make phase1-outbox-recovery-check`
- `make phase1-order-check`

Owner:

- `rippleguard-infra`

## Docs Published Baseline

Status: IN_REVIEW_FOR_MERGE

Evidence:

- Current docs branch records the `VERIFIED` snapshot, remediation history and deferred risks.

Remaining:

- Merge PR #7 after final review.

Owner:

- `rippleguard-docs`

## Phase 1 Contract Scope

Phase 1의 `rippleguard-contracts` 작업은 Phase 0 Published Contract를 다시 설계하거나 덮어쓰는 작업이 아니다.

신규·보완 범위는 다음으로 제한한다.

- Loan Application REST/OpenAPI 신규 계약
- Versioned Snapshot 입력·조회 계약
- 기존 Phase 0 Event의 서비스 적용 Fixture와 DTO 경계
- Outbox·Inbox 처리에 필요한 Header·멱등성 사용 규칙
- Mock Evaluation 서비스 적용 Fixture
- Consumer compatibility 검증

Phase 0 Published Contract는 직접 수정하지 않는다. 호환 가능한 변경은 새 minor schema로 추가하고, 비호환 변경은 새 major generation과 ADR을 요구한다.
