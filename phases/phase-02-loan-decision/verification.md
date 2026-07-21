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

Phase 2 remains `READY` while only the docs planning branch is open. It moves to `IN_PROGRESS` after the planning baseline is merged, a `rippleguard-contracts` Phase 2 PR is opened and that PR records the parent planning commit.

Phase 2 can move to `IN_REVIEW` only after repository implementation PRs are complete and integrated verification evidence exists. Phase 2 can move to `VERIFIED` only after cross-repo baseline, contract versions, image metadata and evidence are published in docs.

## Failure Verification Matrix

| Failure | Injection Point | Expected Status | Retry | State Mutation | Evidence |
| --- | --- | --- | --- | --- | --- |
| Required Feature missing | Agent Runtime input validation | `VALIDATION_REQUIRED` | No | No Loan final state change | invalid fixture log |
| Unsupported extra Feature | Agent Runtime input validation | `VALIDATION_REQUIRED` | No | No Loan final state change | invalid fixture log |
| Feature Schema Version mismatch | Governance or Agent Runtime validation | `VALIDATION_REQUIRED` or `BLOCKED` by conflict condition | No | No accepted proposal | contract test |
| Snapshot ID missing | Governance request handling | `NON_RETRYABLE` | No | No accepted proposal | integration test |
| Snapshot temporary lookup failure | Governance or Agent Runtime lookup | `RETRYABLE` then `VALIDATION_REQUIRED` after exhaustion | Yes | No accepted proposal before success | retry log |
| Snapshot digest mismatch | Agent Runtime validation | `BLOCKED` | No | No accepted proposal | digest test |
| Model Manifest missing | Agent Runtime startup or request validation | `BLOCKED` | No | No accepted proposal | startup/request log |
| Model Binary Artifact digest mismatch | Agent Runtime model load | `BLOCKED` | No | No accepted proposal | digest verification log |
| Agent timeout | Governance request execution | `RETRYABLE` then `VALIDATION_REQUIRED` after exhaustion | Yes | No accepted proposal before success | timeout test |
| Duplicate Agent Request | Governance idempotency handling | Idempotent accepted or pending response | No duplicate execution required | No duplicate accepted result | idempotency test |
| Same `agentRunId` conflicting result | Governance result validation | `BLOCKED` | No | Existing accepted result is not overwritten | conflict test |
| SHAP calculation failure | Agent Runtime explanation step | `VALIDATION_REQUIRED` | Implementation-defined | No accepted proposal unless contract permits partial explanation | failure fixture |
| Audit publish failure | Governance outbox publish | `RETRYABLE`; `BLOCKED` only after durable publication cannot be guaranteed | Yes | Proposal validation state is not lost; audit pending is explicit | outbox test |
| Local LLM Runtime absent | Infra E2E | PASS for Phase 2 | N/A | No dependency on LLM state | compose report |
