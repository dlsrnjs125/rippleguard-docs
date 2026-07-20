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

| Repository | Published Baseline | Candidate Baseline | Completed Scope | Remaining Work | Owner | Blocks | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-contracts` | `29f6c348fd93633476438ee36b3f93a3d036e165` | N/A | Phase 1 REST/OpenAPI, Event schemas, fixtures and compatibility validation | None for Phase 1 baseline | `rippleguard-contracts` | None | VERIFIED_FOR_CONTRACTS |
| `rippleguard-loan-service` | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | N/A | Loan REST, Snapshot persistence, Outbox, Inbox handling, finalization, OCI labels and service tests | Registry digest pinning, GHCR publish, SBOM and build attestation deferred | `rippleguard-loan-service` | None | VERIFIED |
| `rippleguard-governance-service` | `4e06e672affddc02d7e6662f3022d00de86bb3b9` | N/A | Decision Case, Evaluation Run, deterministic Mock Evaluation, stable event ordering, Decision Command, Outbox, OCI labels and service tests | Registry digest pinning, GHCR publish, SBOM and build attestation deferred | `rippleguard-governance-service` | None | VERIFIED |
| `rippleguard-audit-replay-service` | `83ca52edda2f608f90d10694428dff6dffee8a23` | N/A | Event ingestion, deduplication, privacy sanitization, minimal Timeline API, OCI labels and service tests | Registry digest pinning, GHCR publish, SBOM and build attestation deferred | `rippleguard-audit-replay-service` | None | VERIFIED |
| `rippleguard-infra` | `4499fa6a321d5bd1305ec6f07910fbc9c3096db4` | N/A | Compose wiring, Kafka topics, manifest, static validation, commit-tagged image build, OCI verification and Phase 1 runtime PASS evidence | Registry digest pinning, GHCR publish, SBOM, SLSA and release Flyway checksum pinning deferred | `rippleguard-infra` | Docs finalization merge | VERIFIED |
| `rippleguard-docs` | Pending PR #7 merge | PR #7 head branch `docs/phase-1-baseline-finalization` | Phase 1 `VERIFIED`, Phase 2 `READY`, evidence, risk and troubleshooting records prepared on PR #7 | Merge PR #7 after final review | `rippleguard-docs` | None | IN_REVIEW_FOR_MERGE |

## Baseline Decision

Phase 1 `VERIFIED` and Phase 2 `READY` become the published project status when PR #7 is merged to main. The candidate status is backed by GitHub PRs, merge commits, implementation CI, contract validation, service tests, service image provenance and Docker Compose runtime checks.

The initial Phase 1 implementation commits remain historical candidates and are superseded for the published runtime baseline:

| Repository | Superseded Candidate | Superseded By | Reason |
| --- | --- | --- | --- |
| `rippleguard-loan-service` | `54ea344a682723d61d9beedf4ade56ee48029c0d` | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | OCI provenance labels added |
| `rippleguard-governance-service` | `29bafba34c47e003fdefafa455924992993721cf` | `4e06e672affddc02d7e6662f3022d00de86bb3b9` | OCI provenance labels and event ordering remediation added |
| `rippleguard-audit-replay-service` | `e7d9d9f8afb106ecdec16235d79695d88c18b3cd` | `83ca52edda2f608f90d10694428dff6dffee8a23` | OCI provenance labels added |
| `rippleguard-infra` | `ebdd864b279e39701f2bb21750a5080a827943f2` | `4499fa6a321d5bd1305ec6f07910fbc9c3096db4` | Manifest baseline and runtime verification evidence added |

## Repository Follow-up Ownership

Remediation ownership is closed for Phase 1:

- Loan OCI remediation: `rippleguard-loan-service` PR #2, merge `e403c0a60ccb1cebf03380832d047f3fc01019e0`.
- Governance OCI remediation: `rippleguard-governance-service` PR #2, merge `45790ebd5de1c458f87a38b1a067b46c15a59134`.
- Governance event ordering remediation: `rippleguard-governance-service` PR #3, merge `4e06e672affddc02d7e6662f3022d00de86bb3b9`.
- Audit OCI remediation: `rippleguard-audit-replay-service` PR #2, merge `83ca52edda2f608f90d10694428dff6dffee8a23`.
- Infra runtime verification: `rippleguard-infra` PR #3, merge `4499fa6a321d5bd1305ec6f07910fbc9c3096db4`.

Do not weaken Infra verification, manually relabel an unverified image, change only the Infra manifest, or claim registry digest, SBOM, SLSA or build attestation as complete.
