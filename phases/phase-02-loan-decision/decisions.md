# Phase 2 Decisions

## Accepted

| Decision | Rationale |
| --- | --- |
| Contract-First execution | Producer and consumer implementations need stable schemas and fixtures before coding |
| Phase 2 remains tabular ML only | Loan Decision needs reproducibility; Local LLM begins in Phase 3 |
| Loan Proposal is not final Loan Decision | Loan Service owns final state; Governance owns routing |
| `StructuredLlmPort` executable contract is deferred to Phase 3 | Prevents speculative unused LLM interface in Phase 2 |
| Audit scope is minimal Agent Run timeline | Phase 7 replay/hash-chain scope is not pulled forward |
| Phase 2 result flow is Governance-validated | Agent Runtime returns results to Governance; Governance validates and emits Audit trace to avoid duplicate raw result consumers |
| Governance owns `agentRunId` and request idempotency | Retries must address the same logical Agent Run while Agent Runtime records separate attempts |

## Open Questions

| Question | Owner | Resolution Target |
| --- | --- | --- |
| Exact schema field names for Feature Schema, Proposal and Manifest | `rippleguard-contracts` | Contracts PR |
| Numeric tolerance for reproducibility comparison | `rippleguard-agent-runtime` | Agent Runtime PR |
| Transport shape for Agent request/result path | `rippleguard-contracts`, `rippleguard-governance-service`, `rippleguard-agent-runtime` | Contracts and integration PRs |
| Model Binary Artifact storage path: mounted file vs MinIO reference | `rippleguard-agent-runtime`, `rippleguard-infra` | Agent Runtime and Infra PRs |
| Image digest availability for local Phase 2 baseline | `rippleguard-infra` | Infra PR |
| Whether Phase 2 replaces, versions or leaves `agent.evaluation.completed.v1` as Phase 1 mock-only | `rippleguard-contracts` | Contracts PR |
| Exact idempotency key field names and `attemptId` representation | `rippleguard-contracts` | Contracts PR |
