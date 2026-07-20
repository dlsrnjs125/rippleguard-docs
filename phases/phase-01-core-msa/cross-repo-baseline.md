# Phase 1 Cross-Repo Baseline

Phase 1 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 Published Baseline으로 사용하지 않는다.

## Repository Order

1. `rippleguard-contracts`
2. `rippleguard-loan-service`
3. `rippleguard-governance-service`
4. `rippleguard-audit-replay-service`
5. `rippleguard-infra`
6. `rippleguard-docs` finalization

## Baseline Snapshot

| Repository | Published Baseline | Completed Scope | Remaining Work | Owner | Blocks | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-contracts` | `29f6c348fd93633476438ee36b3f93a3d036e165` | Phase 1 REST/OpenAPI, Event schemas, fixtures and compatibility validation | None for Phase 1 baseline | `rippleguard-contracts` | None | VERIFIED_FOR_CONTRACTS |
| `rippleguard-loan-service` | `54ea344a682723d61d9beedf4ade56ee48029c0d` | Loan REST, Snapshot persistence, Outbox, Inbox handling, finalization and service tests | Add OCI revision/source labels in a follow-up PR; merge to main; record that new merge commit as the revised service baseline; build and tag an immutable image from the new merge commit; provide provenance evidence | `rippleguard-loan-service` | Infra image verification and runtime E2E | ACTION_REQUIRED |
| `rippleguard-governance-service` | `29bafba34c47e003fdefafa455924992993721cf` | Decision Case, Evaluation Run, deterministic Mock Evaluation, Decision Command, Outbox and service tests | Add OCI revision/source labels in a follow-up PR; merge to main; record that new merge commit as the revised service baseline; build and tag an immutable image from the new merge commit; provide provenance evidence | `rippleguard-governance-service` | Infra image verification and runtime E2E | ACTION_REQUIRED |
| `rippleguard-audit-replay-service` | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | Event ingestion, deduplication, privacy sanitization, minimal Timeline API and service tests | Add OCI revision/source labels in a follow-up PR; merge to main; record that new merge commit as the revised service baseline; build and tag an immutable image from the new merge commit; provide provenance evidence | `rippleguard-audit-replay-service` | Infra image verification and runtime E2E | ACTION_REQUIRED |
| `rippleguard-infra` | `ebdd864b279e39701f2bb21750a5080a827943f2` | Compose wiring, Kafka topics, manifest, static validation, Phase 1 runtime scripts | Update image baselines after service rebuilds; rerun image verification and all Phase 1 runtime commands; publish sanitized summaries | `rippleguard-infra` after service image PRs | Docs `VERIFIED` transition | BLOCKED |
| `rippleguard-docs` | TBD; current branch `docs/phase-1-baseline-finalization` | Current IN_REVIEW snapshot, evidence, risk and troubleshooting records | Final published baseline and status transition after Infra PASS | `rippleguard-docs` | Phase 2 `READY` transition | IN_REVIEW |

## Baseline Decision

Phase 1 is not `VERIFIED`. GitHub PRs, merge commits, CI, contract validation and service tests are confirmed, but service image provenance failed and Docker Compose runtime checks are blocked behind that failure.

## Repository Follow-up Ownership

### `rippleguard-loan-service`

Required:

- Add `org.opencontainers.image.revision` and `org.opencontainers.image.source` labels in a follow-up service PR.
- Merge the PR to main and record the new merge commit as the revised Loan baseline.
- Build an immutable image from the new merge commit.
- Tag the image using the new merge commit SHA.
- Provide image tag, source commit and `docker inspect` provenance evidence.

### `rippleguard-governance-service`

Required:

- Add `org.opencontainers.image.revision` and `org.opencontainers.image.source` labels in a follow-up service PR.
- Merge the PR to main and record the new merge commit as the revised Governance baseline.
- Build an immutable image from the new merge commit.
- Tag the image using the new merge commit SHA.
- Provide image tag, source commit and `docker inspect` provenance evidence.

### `rippleguard-audit-replay-service`

Required:

- Add `org.opencontainers.image.revision` and `org.opencontainers.image.source` labels in a follow-up service PR.
- Merge the PR to main and record the new merge commit as the revised Audit baseline.
- Build an immutable image from the new merge commit.
- Tag the image using the new merge commit SHA.
- Provide image tag, source commit and `docker inspect` provenance evidence.

### `rippleguard-infra`

Blocked until the three service image baselines are updated.

After unblock:

- Update the Phase 1 manifest if image tags or digests change.
- Rerun `verify-phase1-images.py`.
- Rerun `phase1-check`, `phase1-e2e`, `phase1-duplicate-check`, `phase1-recovery-check`, `phase1-outbox-recovery-check` and `phase1-order-check`.
- Publish sanitized summaries instead of raw logs.

Do not weaken Infra verification, manually relabel an unverified image, change only the Infra manifest, or mark runtime E2E as PASS without provenance.

### `rippleguard-docs`

After Infra PASS:

- Pin new merge commits and image evidence.
- Change Phase 1 to `VERIFIED`.
- Change Phase 2 to `READY`.
