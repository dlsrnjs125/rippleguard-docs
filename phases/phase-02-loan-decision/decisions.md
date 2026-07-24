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
| Governance owns `agentRunId` and request idempotency mapping | The logical request key excludes `agentRunId`; retries reuse the mapped Agent Run while Agent Runtime records separate attempts |
| Loan Feature Snapshot is the Phase 2 feature source of truth | Governance must use Loan Service's immutable snapshot API, not a current Loan Application fallback or synthetic fixture |
| Snapshot identity uses persisted DB precision | Loan Service must return the DB-persisted `createdAt` value in the snapshot entity, reference and API payload so Governance strict identity checks remain meaningful |
| Governance validates Snapshot and Feature Payload digests strictly | Digest mismatches are blocked instead of repaired by retrying latest snapshots or using cached/default payloads |
| Event causation uses predecessor Event ID | Governance validation events set `causationId` to the persisted request event `eventId`, never to `agentRunId` |
| `agentRunId` is a domain/run identity | Audit and Governance keep `agentRunId` as run metadata and do not treat it as an event predecessor |
| Audit uses bounded pending reconciliation for cross-topic causation | Request and validation events are on different Kafka topics, so Audit cannot assume cross-topic delivery order |
| Model artifact ownership stays in Agent Runtime | Agent Runtime owns the model manifest and artifact digest; Infra publishes the release baseline |
| Runtime image digest ownership stays in Infra release evidence | Agent Runtime adds OCI source/revision labels but does not write a build-result image digest back into the embedded model manifest |
| Phase 2 has no Local LLM runtime dependency | Local LLM/Ollama starts in Phase 3; Phase 2 gates verify absence of local or remote LLM wiring |
| Local image ID is local-compose baseline evidence only | Registry digest publication remains productionization follow-up and is not represented as already published |

## Open Questions

| Question | Owner | Resolution Target |
| --- | --- | --- |
| How to implement the ten required failure drills with real runtime injection | `rippleguard-infra` plus service owners | Next Phase 2 verification PR |
| Registry image digest publication and pullable image references | `rippleguard-infra` plus service repositories | Productionization follow-up |
| Whether Audit Agent Run read model should expose non-null Phase 2 provenance metadata | `rippleguard-audit-replay-service`, `rippleguard-contracts` | Follow-up if Phase 2 read API requires those fields |

## Final Review Decisions

| Decision | Rationale |
| --- | --- |
| Phase 2 remains `BLOCKED`, not `VERIFIED` | `make phase2-verify` still fails ten required runtime failure drills with exit code `2` |
| Happy path E2E is now proven | Infra evidence shows Loan submission, immutable Feature Snapshot, Governance evaluation, Agent Runtime inference, validation, Audit timeline and non-final Loan status |
| Local image provenance is now proven for the compose baseline | Service images have local digests/image IDs and OCI source/revision checks; registry digest publication remains follow-up |
| `phase2-scaffold-check` is historical scaffolding evidence only | The final review now relies on `phase2-verify`, `phase2-e2e`, `phase2-verify-images`, reproducibility and local-LLM absence evidence |
| Phase 3·4 handoff is not approved | Failure drill evidence is incomplete even though happy path Decision Envelope, production snapshot path and Audit Timeline are proven |
