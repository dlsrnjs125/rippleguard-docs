# Phase 1 Progress

Phase 1은 implementation merge 상태와 published verification 상태를 분리해서 추적한다. 구현 PR이 병합됐더라도 image provenance와 runtime E2E가 PASS가 아니면 Phase 1은 `VERIFIED`가 아니다.

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

Status: IMPLEMENTED / IMAGE_BASELINE_PENDING

Completed:

- Loan Application REST.
- Versioned Snapshot persistence.
- Outbox publication for `loan.application.submitted.v1` and `loan.decision.finalized.v1`.
- Final decision command application.
- Service tests: `./mvnw test` PASS, 24 tests.

Remaining:

- Add required OCI image labels.
- Rebuild immutable image from merged main commit.
- Provide provenance evidence.

Owner:

- `rippleguard-loan-service`

Blocks:

- `rippleguard-infra` image verification.
- Runtime E2E.

## Governance

Status: IMPLEMENTED / IMAGE_BASELINE_PENDING

Completed:

- Decision Case and Evaluation Run persistence.
- Deterministic Mock Evaluation.
- Decision Command outbox.
- Service tests: `./mvnw test` PASS, 16 tests.

Remaining:

- Add required OCI image labels.
- Rebuild immutable image from merged main commit.
- Provide provenance evidence.

Owner:

- `rippleguard-governance-service`

Blocks:

- `rippleguard-infra` image verification.
- Runtime E2E.

## Audit

Status: IMPLEMENTED / IMAGE_BASELINE_PENDING

Completed:

- Core event ingestion.
- Deduplication and malformed event handling.
- Privacy sanitization.
- Minimal Case Timeline API.
- Service tests: `./mvnw test` PASS, 17 tests.

Remaining:

- Add required OCI image labels.
- Rebuild immutable image from merged main commit.
- Provide provenance evidence.

Owner:

- `rippleguard-audit-replay-service`

Blocks:

- `rippleguard-infra` image verification.
- Runtime E2E.

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

Status: BLOCKED

Evidence:

- `python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` failed.

Root Cause:

- Loan, Governance and Audit service images were built without required OCI source/revision labels.

Remaining:

- Service repository follow-up PRs must add labels and rebuild immutable images.

Owner:

- Primary: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-audit-replay-service`
- Secondary: `rippleguard-infra`

## Runtime E2E

Status: BLOCKED_BY_IMAGE_PROVENANCE

Evidence:

- Runtime commands were not marked PASS after image provenance failed.

Blocked commands:

- `make phase1-check`
- `make phase1-e2e`
- `make phase1-duplicate-check`
- `make phase1-recovery-check`
- `make phase1-outbox-recovery-check`
- `make phase1-order-check`

Owner:

- `rippleguard-infra`, after service image baselines are fixed.

## Docs Published Baseline

Status: PENDING

Evidence:

- Current docs branch records the IN_REVIEW snapshot and blockers.

Remaining:

- Update published baseline after Infra runtime evidence is available.
- Change Phase 1 to `VERIFIED` and Phase 2 to `READY` only after all blockers close.

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
