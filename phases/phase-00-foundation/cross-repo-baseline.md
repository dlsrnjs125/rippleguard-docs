# Phase 0 Cross-Repo Baseline

Phase 0 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 baseline으로 사용하지 않는다.

| Repository | Branch | PR | Merge Commit SHA | Contract Version | Docker 또는 Platform Version | Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-docs` | `main` | `#2` | `9738d69693ab825fc1b3b9752be9151505ff4598` | N/A | N/A | 문서 링크·의미 검증, Phase tracking scaffold | CURRENT_DOCS_BASELINE |
| `rippleguard-contracts` | `main` | `#2` | `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Event/Schema baseline in `1.0.0`; `loan.application.submitted.v1` latest compatible schema `1.1.0` | N/A | `make validate`; GitHub Actions `Contract validation` run #8 success | VERIFIED |
| `rippleguard-infra` | `main` | `#1` | `22db5e4fcc435d2492ad10bf33c18618ecc46282` | Pins contracts merge commit `a44f39d819fff5fe79db27e236a42e2a861a8b5e` | Docker Compose local platform: Kafka KRaft, Kafka UI, Loan/Governance PostgreSQL, OPA, MinIO | `make validate-static`, `make validate-contract-baseline`, `make platform-up`, `make platform-check`, `make platform-down`; GitHub CI status not found via status/workflow API | PENDING_CI_CONFIRMATION |
| `rippleguard-loan-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-governance-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-agent-runtime` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-audit-replay-service` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-web` | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |

이 문서 변경 PR 자체의 merge commit은 아직 알 수 없으므로 `rippleguard-docs`의 최종 Phase 0 baseline은 이 PR 병합 후 별도 기록이 필요하다. Infra의 로컬 검증은 통과했지만 GitHub API에서 CI run/status를 확인하지 못했으므로 Phase 0 전체 상태는 아직 `VERIFIED`로 전환하지 않는다.
