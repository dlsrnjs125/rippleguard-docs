# Phase 2 Verification

## Verification Matrix

| 검증 항목 | Owner Repo | 검증 방법 | 성공 조건 | Evidence |
| --- | --- | --- | --- | --- |
| Feature Schema validation | `rippleguard-contracts` | schema test | valid pass, invalid reject | test log |
| Snapshot reference contract | `rippleguard-contracts` | fixture and compatibility test | stable reference or digest accepted | fixture report |
| Loan Proposal contract | `rippleguard-contracts` | schema and examples | proposal is distinct from final decision | schema log |
| Tabular Model Manifest | `rippleguard-contracts` | schema and invalid fixtures | required provenance fields enforced | test log |
| Model reproducibility | `rippleguard-agent-runtime` | repeated inference | same inputs match within tolerance | report |
| SHAP provenance | `rippleguard-agent-runtime` | manifest/result comparison | explainer version and contribution refs linked | fixture |
| Governance validation | `rippleguard-governance-service` | integration test | invalid output not adopted | test output |
| Timeout and retry | `rippleguard-governance-service`, `rippleguard-agent-runtime` | failure test | status and retry exhaustion explicit | log |
| Agent timeline | `rippleguard-audit-replay-service` | API/DB test | Agent Run metadata queryable | response |
| Docker integration | `rippleguard-infra` | compose E2E | Local LLM absent and Phase 2 passes | report |
| Baseline provenance | `rippleguard-docs` | cross-repo review | PR, commit, image and digest linked | baseline doc |

## Evidence Requirements

Evidence must record:

- Repository
- Branch
- PR
- Commit SHA
- Execution command
- Test result
- Artifact path
- Image tag
- Image digest when available
- Execution timestamp
- Known limitations

## Status Rule

Phase 2 can move to `IN_REVIEW` only after repository implementation PRs are complete and integrated verification evidence exists. Phase 2 can move to `VERIFIED` only after cross-repo baseline, contract versions, image metadata and evidence are published in docs.
