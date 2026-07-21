# Contract Version Matrix

| Contract | Phase | Producer | Kafka Consumer | Internal Handler | Schema Version | Producer Implementation | Consumer Implementation | Integration Verification | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| External Risk Signal | 0/1 | FDS Simulator | N/A | Loan Snapshot mapper | `1.0.0` | OUT_OF_SCOPE_FOR_PHASE | OUT_OF_SCOPE_FOR_PHASE | NOT_RUN | CONTRACT_READY |
| Loan Application REST | 1 | Web / test client | N/A | Loan Service | OpenAPI `phase-1-core-msa.v1.0.0`; REST schemas `1.0.0` | Test client covered | Loan handler implemented | Service tests PASS; Compose REST E2E PASS | RUNTIME_VERIFIED |
| Loan Application Status | 1 | Loan Service | N/A | Loan Service | `1.0.0` | Implemented | N/A | Service tests PASS | IMPLEMENTED |
| Decision Case Status | 1 | Governance Service | N/A | Governance Service | `1.0.0` | Implemented | N/A | Service tests PASS | IMPLEMENTED |
| `loan.application.submitted.v1` | 1 | Loan Service | Governance Service, Audit Replay Service | Governance creates Decision Case; Audit records event | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| `governance.review.started.v1` | 1 | Governance Service | Loan Service, Audit Replay Service | Loan review transition; Audit records event | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| `governance.evidence.requested.v1` | 1 deferred path | Governance Service | Loan Service | Loan evidence-required transition | schema `1.0.0`, compatible minor `1.1.0` | Deferred in Phase 1 core flow | Loan consumer capability exists | Runtime not in core manifest | CONTRACT_READY / GOVERNANCE_PRODUCER_DEFERRED |
| `loan.evidence.updated.v1` | 1 deferred path | Loan Service | Governance Service | Governance reassessment | schema `1.0.0`, compatible minor `1.1.0` | Loan producer capability exists | Governance consumer deferred | Runtime not in core manifest | PARTIALLY_IMPLEMENTED |
| `agent.evaluation.requested.v1` | 1 | Governance Service | Audit Replay Service | Audit records requested event | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented by Audit | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| `agent.evaluation.completed.v1` | 1 Mock | Governance Service | Audit Replay Service | Governance Mock evaluator produces result internally before outbox | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented by Audit | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| `agent.evaluation.completed.v1` | 2+ Actual | Agent Runtime | Governance Service, Audit Replay Service | Agent Runtime | schema `1.0.0`, compatible minor `1.1.0` | Deferred | Deferred for Governance; Audit contract ready | NOT_RUN | CONTRACT_READY / IMPLEMENTATION_DEFERRED |
| `loan.decision.commanded.v1` | 1 | Governance Service | Loan Service, Audit Replay Service | Loan finalizes decision; Audit records command | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| `loan.decision.finalized.v1` | 1 | Loan Service | Audit Replay Service | Audit records finalization | schema `1.0.0`, compatible minor `1.1.0` | Implemented | Implemented by Audit | Service tests PASS; runtime E2E PASS | RUNTIME_VERIFIED |
| Decision Envelope | 1 Mock / 2+ Actual | Governance Mock evaluator; later Agent Runtime | N/A | Governance Service | `1.0.0` | Phase 1 Mock implemented | N/A | Deterministic mock evaluation tests PASS; runtime E2E PASS for mock boundary | RUNTIME_VERIFIED_FOR_MOCK |
| Evaluation Run | 1 | Governance Service | N/A | Governance Service | `1.0.0`, `2.0.0` schema available | Implemented as service-local persisted run | N/A | Service tests PASS | IMPLEMENTED |
| Tabular Model Manifest Schema | 2 | N/A; owner `rippleguard-contracts` | Agent Runtime, Governance Service, Audit Replay Service, Infra | Framework, feature schema, preprocessing, dataset, threshold and explainer metadata contract | TBD | N/A | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Tabular Model Manifest Instance | 2 | `rippleguard-agent-runtime`; published baseline by `rippleguard-infra` | Governance Service, Audit Replay Service | XGBoost/LightGBM version, artifact digest, feature schema, preprocessing, training dataset, training code commit, random seed, threshold and SHAP config | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Consequence Envelope | 3 | Agent Runtime | Governance Service | TBD | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Evidence & Control Findings | 4 | Agent Runtime | Governance Service | TBD | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Model Manifest Schema | 3 | N/A; owner `rippleguard-contracts` | Agent Runtime, Infra, Audit Replay Service | Schema validation, version rules, examples and invalid fixtures | TBD | N/A | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Model Manifest Instance | 3 | `rippleguard-agent-runtime`; published baseline by `rippleguard-infra` | Governance Service, Audit Replay Service | Model name, provider, quantization, digest, runtime version, context limit, license and source metadata | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Agent Run Metadata | 2+ | Agent Runtime | Governance Service, Audit Replay Service | Run metadata persistence | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Local LLM Invocation Metadata | 3+ | Agent Runtime | Audit Replay Service | Agent Run Timeline | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Agent Failure Event | 3 | Agent Runtime | Governance Service, Audit Replay Service | Failure code taxonomy, retryable classification, Governance route mapping and Audit trace | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Structured Consequence Envelope | 3 | Agent Runtime | Governance Service | Consequence validator | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Structured Evidence Finding | 4 | Agent Runtime | Governance Service | Evidence validator | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Assurance Policy Input | 5 | Governance Service | OPA | TBD | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Sample Audit Escape Event | 7 | Audit Replay Service | Governance Service | TBD | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |
| Impacted Evaluation Run Query | 7 | Governance Service | Audit Replay Service | TBD | TBD | NOT_STARTED | NOT_STARTED | NOT_RUN | NOT_STARTED |

Contract existence, producer implementation, consumer implementation and integration verification are separate states. A contract can be `CONTRACT_READY` even when one or both implementation sides are deferred.

Model Manifest Schema is an executable cross-repo contract owned by `rippleguard-contracts`. Model Manifest Instance is runtime configuration and baseline evidence owned by `rippleguard-agent-runtime`, then published and verified by `rippleguard-infra` when it becomes part of an integration baseline.

Tabular Model Manifest follows the same schema/instance split. Schema belongs to `rippleguard-contracts`; Phase 2 instances belong to `rippleguard-agent-runtime` and become published runtime baselines only through `rippleguard-infra`.

Agent Failure Event must preserve causes such as `MODEL_RUNTIME_UNAVAILABLE`, `MODEL_LOAD_FAILED`, `MODEL_DIGEST_MISMATCH`, `INFERENCE_TIMEOUT`, `CONTEXT_BUDGET_EXCEEDED`, `MALFORMED_STRUCTURED_OUTPUT`, `SCHEMA_VALIDATION_FAILED` and `DETERMINISTIC_VALIDATION_FAILED`, even when Governance maps them to the same high-level `VERIFICATION_REQUIRED` route.
