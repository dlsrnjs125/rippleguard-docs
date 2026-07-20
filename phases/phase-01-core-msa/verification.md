# Phase 1 Verification

## 검증 목표

AI 없는 Core MSA 최소 수직 흐름이 계약, 상태 소유권, 멱등성, 독립 DB, commit-tagged OCI Image baseline과 Audit Foundation 기준을 만족하는지 검증한다.

## 환경

| 항목 | 값 |
| --- | --- |
| 실행 시각 | 2026-07-20 15:26:52 KST |
| 관련 Commit | Contracts `29f6c348fd93633476438ee36b3f93a3d036e165`; Loan `e403c0a60ccb1cebf03380832d047f3fc01019e0`; Governance `4e06e672affddc02d7e6662f3022d00de86bb3b9`; Audit `83ca52edda2f608f90d10694428dff6dffee8a23`; Infra `4499fa6a321d5bd1305ec6f07910fbc9c3096db4` |
| 환경·도구 버전 | macOS 26.5.1, Java 17.0.19, Docker Desktop 29.5.3, GitHub API |

## Initial Verification

| 항목 | 결과 | 책임 Repository | 보존 이유 |
| --- | --- | --- | --- |
| Loan image provenance | FAIL — OCI labels missing | `rippleguard-loan-service` | Initial Phase 1 implementation image did not expose required source/revision labels |
| Governance image provenance | FAIL — OCI labels missing | `rippleguard-governance-service` | Initial Phase 1 implementation image did not expose required source/revision labels |
| Audit image provenance | FAIL — OCI labels missing | `rippleguard-audit-replay-service` | Initial Phase 1 implementation image did not expose required source/revision labels |
| Runtime duplicate/timeline check | FAIL/BLOCKED — invalid causation ordering observed after partial runtime run | `rippleguard-governance-service` | Governance event ordering remediation was required |

## Remediation

| Repository | PR | Merge Commit | Result |
| --- | --- | --- | --- |
| `rippleguard-loan-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-loan-service/pull/2) | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | OCI labels added |
| `rippleguard-governance-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/2) | `45790ebd5de1c458f87a38b1a067b46c15a59134` | OCI labels added |
| `rippleguard-governance-service` | [PR #3](https://github.com/dlsrnjs125/rippleguard-governance-service/pull/3) | `4e06e672affddc02d7e6662f3022d00de86bb3b9` | Event ordering stabilized |
| `rippleguard-audit-replay-service` | [PR #2](https://github.com/dlsrnjs125/rippleguard-audit-replay-service/pull/2) | `83ca52edda2f608f90d10694428dff6dffee8a23` | OCI labels added |
| `rippleguard-infra` | [PR #3](https://github.com/dlsrnjs125/rippleguard-infra/pull/3) | `4499fa6a321d5bd1305ec6f07910fbc9c3096db4` | Manifest and runtime evidence updated |

## Reverification Matrix

| 항목 | 명령·방법 | 결과 | 상태 |
| --- | --- | --- | --- |
| Contract validation | `rippleguard-contracts make validate` | 30 schemas, 1 OpenAPI, examples, scenarios and compatibility checks passed | PASS |
| Loan service tests | `rippleguard-loan-service ./mvnw test` | 24 tests passed | PASS |
| Governance service tests | `rippleguard-governance-service make test` | 17 tests passed | PASS |
| Audit service tests | `rippleguard-audit-replay-service make test` | 17 tests passed | PASS |
| Infra static validation | `rippleguard-infra make validate-static` | Compose config, topic baseline, manifest and secret checks passed | PASS |
| Contract baseline validation | `rippleguard-infra make validate-contract-baseline` | Contracts schemas, baseline manifest and Kafka topics matched | PASS |
| Image build and provenance | `rippleguard-infra make phase1-build-images` | All service images built from clean matching checkouts and labels verified | PASS |
| Image provenance verification | `rippleguard-infra make phase1-verify-images` | Manifest image tag, revision label and source label matched | PASS |
| Phase 1 compose check | `rippleguard-infra make phase1-check` | Health, topics and Flyway migrations verified | PASS |
| Runtime E2E | `rippleguard-infra make phase1-e2e` | Application reached final cross-service flow and timeline privacy validation passed | PASS |
| Duplicate test | `rippleguard-infra make phase1-duplicate-check` | Duplicate idempotency and replay consumers verified | PASS |
| Recovery test | `rippleguard-infra make phase1-recovery-check` | Governance and Audit restart recovery verified | PASS |
| Outbox recovery test | `rippleguard-infra make phase1-outbox-recovery-check` | Stored outbox event published after publisher restart | PASS |
| Timeline order test | `rippleguard-infra make phase1-order-check` | Delayed/out-of-order event arrival handled without duplicate canonical events | PASS |
| Shutdown | `rippleguard-infra make phase1-down` | Compose stack removed | PASS |

## Image Labels

| Service | Image | Revision Label | Source Label |
| --- | --- | --- | --- |
| Loan | `rippleguard-loan-service:e403c0a60ccb` | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | `https://github.com/dlsrnjs125/rippleguard-loan-service` |
| Governance | `rippleguard-governance-service:4e06e672affd` | `4e06e672affddc02d7e6662f3022d00de86bb3b9` | `https://github.com/dlsrnjs125/rippleguard-governance-service` |
| Audit | `rippleguard-audit-replay-service:83ca52edda2f` | `83ca52edda2f608f90d10694428dff6dffee8a23` | `https://github.com/dlsrnjs125/rippleguard-audit-replay-service` |

## Migration Evidence

| Service | Version | Description | Script | Runtime Checksum |
| --- | --- | --- | --- | --- |
| Loan | `1` | `loan core` | `V1__loan_core.sql` | `981324118` |
| Governance | `1` | `governance core` | `V1__governance_core.sql` | `-1338716243` |
| Audit | `1` | `audit foundation` | `V1__audit_foundation.sql` | `51402219` |

## 잔여 위험

- OCI labels declare source repository and revision but do not replace registry digest pinning or build attestation.
- `imageDigest` remains `null` in the Infra manifest.
- Registry Digest, GHCR Publish, SBOM, Build Attestation and SLSA Provenance are deferred.
- Release Flyway checksum pinning is deferred; runtime Flyway version, description, script and checksum evidence are recorded.
- OPA policy integration, Replay, Hash Chain, Version Diff, Execution Graph and Graph UI remain outside Phase 1.
