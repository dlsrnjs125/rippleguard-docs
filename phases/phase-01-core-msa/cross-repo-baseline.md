# Phase 1 Cross-Repo Baseline

Phase 1 baseline은 병합된 main commit만 게시 기준으로 사용한다. 작업 브랜치 HEAD나 `latest`는 Published Baseline으로 사용하지 않는다.

## Repository Order

1. `rippleguard-contracts`
2. `rippleguard-loan-service`
3. `rippleguard-governance-service`
4. `rippleguard-audit-replay-service`
5. `rippleguard-infra`
6. `rippleguard-docs` finalization

현재 Repository PR이 main에 병합된 뒤 다음 Repository 작업을 시작한다. 한 번에 하나의 Repository만 수정한다. 각 서비스 상세 구현은 해당 Repository에서 관리하고, 중앙 Docs에는 Cross-Repo 진행 상태와 검증 Baseline만 기록한다.

| Repository | Branch | PR | Candidate Baseline | Published Baseline | Docker Image | Contract Version | DB Migration | Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-docs` | `docs/phase-1-kickoff` | PR candidate | PR Head | TBD | N/A | N/A | N/A | `git diff --check`, Markdown link check | IN_REVIEW |
| `rippleguard-contracts` | TBD | TBD | TBD | TBD | N/A | TBD | N/A | NOT_STARTED | NOT_STARTED |
| `rippleguard-loan-service` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-governance-service` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-audit-replay-service` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |
| `rippleguard-infra` | TBD | TBD | TBD | TBD | TBD | TBD | TBD | NOT_STARTED | NOT_STARTED |

PR 중에는 Candidate Baseline을 `PR Head`로 표시하고 `IN_REVIEW`로 관리한다. 병합 뒤 별도 metadata update에서 Merge Commit SHA 또는 승인된 Phase Tag를 Published Baseline으로 고정한 뒤에만 `VERIFIED`로 올린다.
