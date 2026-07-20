# Phase 1 Evidence

Phase 1 Evidence에는 구현 Repository별 PR, 병합 commit, 검증 명령, CI Run, Docker Compose runtime 결과와 Baseline 값을 기록한다.

Phase 1은 initial verification에서 image provenance와 event ordering 문제가 확인됐고, 보완 PR 병합 후 Infra runtime reverification이 PASS로 완료되어 `VERIFIED`다.

## Confirmed Evidence

| Repository | Evidence | Result |
| --- | --- | --- |
| `rippleguard-contracts` | [PR #3](https://github.com/dlsrnjs125/rippleguard-contracts/pull/3), merge `29f6c348fd93633476438ee36b3f93a3d036e165`, GitHub check `validate` | PASS |
| `rippleguard-contracts` | `make validate`: 30 schemas, 1 OpenAPI document, 50 valid examples, 78 intentional invalid examples, 8 scenarios, 16 fixture-backed compatibility checks | PASS |
| `rippleguard-loan-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/1), merge `54ea344a682723d61d9beedf4ade56ee48029c0d`, GitHub check `test-package-docker` | PASS / SUPERSEDED_FOR_IMAGE_BASELINE |
| `rippleguard-loan-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/2), merge `e403c0a60ccb1cebf03380832d047f3fc01019e0`, OCI labels added | PASS |
| `rippleguard-governance-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/1), merge `29bafba34c47e003fdefafa455924992993721cf`, GitHub check `test-package-docker` | PASS / SUPERSEDED_FOR_RUNTIME_BASELINE |
| `rippleguard-governance-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/2), merge `45790ebd5de1c458f87a38b1a067b46c15a59134`, OCI labels added | PASS / SUPERSEDED_BY_ORDERING_FIX |
| `rippleguard-governance-service` | [PR #3](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/3), merge `4e06e672affddc02d7e6662f3022d00de86bb3b9`, event ordering fixed | PASS |
| `rippleguard-audit-replay-service` | [PR #1](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/1), merge `e7d9d9f8afb106ecdec16235d79695d88c18b3cd`, GitHub check `test` | PASS / SUPERSEDED_FOR_IMAGE_BASELINE |
| `rippleguard-audit-replay-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/2), merge `83ca52edda2f608f90d10694428dff6dffee8a23`, OCI labels added | PASS |
| `rippleguard-infra` | [PR #2](https://github.com/dlsrnjs125/rippleguard-infra/pull/2), merge `ebdd864b279e39701f2bb21750a5080a827943f2`, GitHub check `static-validation` | PASS / SUPERSEDED_FOR_RUNTIME_BASELINE |
| `rippleguard-infra` | [PR #3](https://github.com/dlsrnjs125/rippleguard-infra/pull/3), merge `4499fa6a321d5bd1305ec6f07910fbc9c3096db4`, runtime verification evidence | PASS |
| `rippleguard-docs` | PR #7 head branch `docs/phase-1-baseline-finalization` | IN_REVIEW_FOR_MERGE |

## Docs Verification Boundary

Local verification:

- `git diff --check`: PASS
- Markdown relative link check: PASS

GitHub Actions:

- Not configured for Docs PR #7.

The local checks are valid PR evidence, but they are not recorded as GitHub Actions PASS.

## Initial Missing Evidence

| Verification | Initial Result | Owner | Resolution |
| --- | --- | --- | --- |
| Loan OCI provenance | Required labels absent | `rippleguard-loan-service` | PR #2 added OCI labels and Infra image verification passed |
| Governance OCI provenance | Required labels absent | `rippleguard-governance-service` | PR #2 added OCI labels and PR #3 supplied final runtime baseline |
| Audit OCI provenance | Required labels absent | `rippleguard-audit-replay-service` | PR #2 added OCI labels and Infra image verification passed |
| Runtime E2E | Blocked by image provenance | `rippleguard-infra` | PR #3 reran full Phase 1 runtime verification |
| Duplicate and timeline order | Runtime failure observed before Governance ordering fix | `rippleguard-governance-service` | PR #3 stabilized event ordering; Infra PR #3 passed duplicate and order checks |

## Runtime Verification Evidence

| Command | Result |
| --- | --- |
| `make validate-static` | PASS |
| `make validate-contract-baseline` | PASS |
| `make phase1-build-images` | PASS |
| `make phase1-verify-images` | PASS |
| `make phase1-up` | PASS |
| `make phase1-check` | PASS |
| `make phase1-e2e` | PASS |
| `make phase1-duplicate-check` | PASS |
| `make phase1-recovery-check` | PASS |
| `make phase1-outbox-recovery-check` | PASS |
| `make phase1-order-check` | PASS |
| `make phase1-down` | PASS |

Infra evidence file:

- `rippleguard-infra/docs/evidence/phase-1-runtime-verification.md`

## Image Manifest Snapshot

| Service | Image Tag | Digest | Expected OCI Revision | Migration |
| --- | --- | --- | --- | --- |
| Loan | `rippleguard-loan-service:e403c0a60ccb` | `null` in manifest | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | `V1__loan_core.sql` |
| Governance | `rippleguard-governance-service:4e06e672affd` | `null` in manifest | `4e06e672affddc02d7e6662f3022d00de86bb3b9` | `V1__governance_core.sql` |
| Audit | `rippleguard-audit-replay-service:83ca52edda2f` | `null` in manifest | `83ca52edda2f608f90d10694428dff6dffee8a23` | `V1__audit_foundation.sql` |

## Deferred Evidence

- Registry Digest
- GHCR Publish
- SBOM
- Build Attestation
- SLSA Provenance
- Release Flyway Checksum Pinning

Logs should be summarized, not copied wholesale. Evidence must be enough to reproduce the verification decision without exposing raw application payloads or secrets.
