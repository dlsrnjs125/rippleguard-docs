# Phase 0 Cross-Repo Baseline

Phase 0 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 baseline으로 사용하지 않는다.

| Repository | Branch | PR | Merge Commit SHA | Contract Version | Docker 또는 Platform Version | Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-docs` | `docs/phase-0-baseline-finalization` | `#4` | Pending; candidate head `f93108ddee238877875565c04589c48072e46d6f` | N/A | N/A | 문서 링크·의미 검증, Phase 0 tracking finalization candidate | IN_REVIEW |
| `rippleguard-contracts` | `main` | `#2` | `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Event/Schema baseline in `1.0.0`; `loan.application.submitted.v1` latest compatible schema `1.1.0` | N/A | `make validate`; GitHub Actions `Contract validation` run #8 success | VERIFIED |
| `rippleguard-infra` | `main` | `#1` | `22db5e4fcc435d2492ad10bf33c18618ecc46282` | Pins contracts merge commit `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Docker Compose local platform: Kafka KRaft, Kafka UI, Loan/Governance PostgreSQL, OPA, MinIO | GitHub Actions `Infra validation` run #5 success on head `06e6f30fb20bdc121b163699acdb33330cafd0b8`; local `platform-up/check/down` PASS | VERIFIED |
| `rippleguard-loan-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-governance-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-agent-runtime` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-audit-replay-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-web` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |

이 문서 변경 PR #4 자체의 merge commit은 아직 알 수 없으므로 `rippleguard-docs`의 최종 Phase 0 baseline은 이 PR 병합 후 Metadata PR에서 고정한다. 이 원칙 때문에 Phase 0은 현재 `IN_REVIEW`이며, PR #4 merge commit 고정 후 `VERIFIED`로 전환한다.
