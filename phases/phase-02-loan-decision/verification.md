# Phase 2 Verification

Final cross-repository verification on 2026-07-22 concluded `BLOCKED`. See [Final Cross-Repository Verification](final-review.md).

## Verification Matrix

| 검증 항목 | Owner Repo | Baseline | 명령·방법 | 실제 결과 | Evidence | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Contracts | `rippleguard-contracts` | `f4012e8b5a0dcd5605b61652a5c39deacb14454b` | `PYTHONDONTWRITEBYTECODE=1 make validate` | PASS | Direct command output in final review | PASS |
| Reproducibility | `rippleguard-agent-runtime` | `35121627550e5999a084c123b610f47884aa01f7` | `make reproducibility-test` with pytest cache disabled | PASS for one integration test | Direct command output in final review | PASS_WITH_LIMITATIONS |
| SHAP | `rippleguard-agent-runtime` | `35121627550e5999a084c123b610f47884aa01f7` | Source/test inspection | Not directly executed in final review | Source tests | UNVERIFIED |
| Governance validation | `rippleguard-governance-service` | `6e5dee34a01429ac017774fcd7a238e2f1415481` | Source/test inspection | Not directly executed in final review | Source tests | UNVERIFIED |
| Retry·timeout | `governance/runtime` | `6e5dee3`, `3512162` | Source/test inspection | No integrated drill executed | Source tests | UNVERIFIED |
| Audit Timeline | `rippleguard-audit-replay-service` | `f3162d3bf3ea2bcfd8d40c929607a84d88054082` | Source/test inspection | No full E2E API result | Source tests | UNVERIFIED |
| Image provenance | `rippleguard-infra` | `b4051c271fe3b82cb8b6d2b8b89517f98017165c` | `python3 scripts/verify-phase2-images.py` | FAIL | Images not inspectable; manifest image digests are null | BLOCKED |
| Full E2E | `rippleguard-infra` | `b4051c2` | `./scripts/phase2-e2e.sh` | BLOCKED | Direct command output | BLOCKED |
| Local LLM absent | `rippleguard-infra`, `rippleguard-contracts` | `b4051c2`, `f4012e8` | Fixture/source inspection | No Phase 2 LLM dependency observed, no full E2E PASS | Source/evidence | PASS_WITH_LIMITATIONS |
| Artifact failure | `runtime/infra` | model digest baseline | Source/test inspection | No integrated drill executed | Source tests | UNVERIFIED |

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

Phase 2 is `BLOCKED` until full production E2E, image digest provenance, and Loan/Governance feature snapshot integration are proven. `PASS_WITH_LIMITATIONS` does not allow promotion to `VERIFIED`.

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
| SHAP calculation failure | Agent Runtime explanation step | `VALIDATION_REQUIRED` | No | Score-only output is not adopted as normal completion; partial result is debugging/evidence only | failure fixture |
| Audit publish failure | Governance outbox publish | `RETRYABLE`; `BLOCKED` only after durable publication cannot be guaranteed | Yes | Proposal validation state is not lost; audit pending is explicit | outbox test |
| Local LLM Runtime absent | Infra E2E | PASS for Phase 2 | N/A | No dependency on LLM state, but full E2E is currently blocked | compose/source report |
