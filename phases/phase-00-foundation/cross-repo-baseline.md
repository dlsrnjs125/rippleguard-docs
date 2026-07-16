# Phase 0 Cross-Repo Baseline

Phase 0 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 baseline으로 사용하지 않는다.

| Repository | Branch | PR | Merge Commit SHA | Contract Version | Docker 또는 Platform Version | Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-docs` | `main` | `#4` | `d729e21c62a2372ea044c6f68fe827595b266483` | N/A | N/A | 문서 링크·의미 검증, Phase 0 tracking finalization merged baseline | VERIFIED |
| `rippleguard-contracts` | `main` | `#2` | `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Event/Schema baseline in `1.0.0`; `loan.application.submitted.v1` latest compatible schema `1.1.0` | N/A | `make validate`; GitHub Actions `Contract validation` run #8 success | VERIFIED |
| `rippleguard-infra` | `main` | `#1` | `22db5e4fcc435d2492ad10bf33c18618ecc46282` | Pins contracts merge commit `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Docker Compose local platform: Kafka KRaft, Kafka UI, Loan/Governance PostgreSQL, OPA, MinIO | GitHub Actions `Infra validation` run #5 success on head `06e6f30fb20bdc121b163699acdb33330cafd0b8`; local `platform-up/check/down` PASS | VERIFIED |
| `rippleguard-loan-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-governance-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-agent-runtime` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-audit-replay-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-web` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |

Phase 0 Published Baseline은 Contracts PR #2, Infra PR #1, Docs PR #4의 병합된 main commit을 기준으로 고정한다. Branch나 변경 가능한 `latest`는 Published Baseline으로 사용하지 않는다.
