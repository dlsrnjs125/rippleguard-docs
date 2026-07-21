# Phase 2 Contract Plan

## Contract-First Rules

- Schema is finalized before producer implementation.
- Consumers must be able to develop from schemas and example fixtures.
- Loan Proposal is not a final Loan Decision.
- Decision Envelope includes model reliability and provenance metadata.
- Tabular Model Manifest cannot be only a model name.
- Agent Run must be traceable to Evaluation Run and Decision Case.
- Failure must not be hidden by default values or mock success responses.
- Schema Version and Model Version are tracked independently.

## Phase 2 Event Ownership

Phase 2 uses a Governance-validated result flow.

```text
Governance Service -> Agent Runtime
Agent Runtime -> Governance Service
Governance Service -> Audit Replay Service
```

Agent Runtime does not publish the same Phase 2 result directly to Audit Replay. Governance validates the Agent output first, then emits a validated or rejected audit event. `agent.evaluation.completed.v1` remains the Phase 1 mock completion event unless `rippleguard-contracts` explicitly versions or replaces it.

## Agent Run Identity and Idempotency

Docs define the intended ownership; executable field names are finalized by `rippleguard-contracts`.

```text
decisionCaseId
  -> evaluationRunId
       -> agentRunId
            -> attemptId: 1
            -> attemptId: 2
            -> attemptId: 3
```

- Governance owns `evaluationRunId`, `agentRunId` and the request idempotency key.
- Agent Runtime owns execution attempt metadata, including `attemptId`.
- Retrying the same Agent Request reuses the same `agentRunId`.
- A new execution attempt under the same request receives a new `attemptId`.
- The idempotency key is derived from the fixed Decision Case, Evaluation Run, Agent Run, Snapshot reference, Feature Schema Version, Model Version, Model Artifact Digest and Threshold Version.
- The same `agentRunId` with a different Snapshot, Feature Schema, Model Version, Artifact Digest or Threshold Version is a contract validation failure.
- Conflicting successful results for the same `agentRunId` are `BLOCKED`.
- Duplicate delivery of the same result is idempotent and must not create a second accepted result.

## Backward Compatibility Rules

| Change | Compatibility | Version Handling |
| --- | --- | --- |
| Optional field addition | Backward compatible when consumers ignore unknown fields | Minor |
| Required field addition | Breaking | Major |
| Existing field rename or removal | Breaking | Major |
| Existing enum value removal or semantic change | Breaking | Major |
| Enum value addition | Consumer policy required; unknown values must not be treated as success | Minor only when consumers have an explicit unknown-value path |
| Unknown field | Ignore only when schema declares extension tolerance; otherwise reject | Schema-defined |
| Unknown failure code | Map to validation-required or blocked, never success | Minor only with safe fallback mapping |
| Event name version change | New event/topic or explicit routing rule required | Major or new event |
| Phase 1 mock result to Phase 2 Decision Envelope migration | No implicit migration; contracts must define whether reuse, v2 extension or replacement applies | Contracts PR decision |
| Previous-version fixture | Required for any compatible change claim | Minor/Major evidence |

## Contract Candidates

| Contract | Owner | Phase 2 Meaning | Notes |
| --- | --- | --- | --- |
| Feature Schema | `rippleguard-contracts` | Versioned tabular features accepted by Loan Decision Agent | Includes valid/invalid fixtures |
| Versioned Snapshot Reference | `rippleguard-contracts` | Stable reference or content digest for financial Snapshot | Field names finalized in contracts |
| Loan Proposal | `rippleguard-contracts` | Agent-generated recommendation and scores | Not final approval/rejection |
| Decision Envelope | `rippleguard-contracts` | Proposal plus input, provenance, evidence refs and model metadata | Extends Phase 1 mock boundary |
| Tabular Model Manifest Schema | `rippleguard-contracts` | Required metadata for tabular model reproducibility | Instance owned by Agent Runtime |
| Agent Run Metadata | `rippleguard-contracts` | `agentRunId`, status, timing, version refs, failure summary | Must connect to Evaluation Run |
| Agent Request Event/API | `rippleguard-contracts` | Governance request for fixed Snapshot/model version | Transport finalized in implementation |
| Agent Result Event/API | `rippleguard-contracts` | Agent output or failure response | Must include correlation and causation metadata where applicable |
| Failure Reason Code | `rippleguard-contracts` | Classification for validation/runtime/model failures | Exact enum values TBD |

## Minimum Tabular Manifest Semantics

Docs define required meaning; executable field names are finalized by `rippleguard-contracts`.

- `modelName`
- `modelVersion`
- `framework`
- `frameworkVersion`
- `featureSchemaVersion`
- `preprocessingVersion`
- `trainingDatasetReference`
- `evaluationDatasetVersion`
- `trainingCodeCommit`
- `randomSeed`
- `thresholdVersion`
- `modelBinaryArtifactReference`
- `modelBinaryArtifactDigest`
- `shapExplainerVersion`
- `shapExplainerConfig`
- `createdAt`

## Failure Scenarios

| Scenario | Expected Classification |
| --- | --- |
| Feature Schema Version mismatch | `VALIDATION_REQUIRED` when the request references an unsupported version; `BLOCKED` when a published baseline resolves to conflicting schemas |
| Required Feature missing | `VALIDATION_REQUIRED` |
| Unsupported extra Feature | `VALIDATION_REQUIRED` |
| Snapshot lookup failure | `RETRYABLE` for temporary storage/network failure; `NON_RETRYABLE` when the Snapshot ID does not exist; `BLOCKED` on digest mismatch |
| Model Manifest missing | `BLOCKED` |
| Model Binary Artifact missing | `BLOCKED` |
| Model Binary Artifact digest mismatch | `BLOCKED` |
| Agent timeout | `RETRYABLE`, then `VALIDATION_REQUIRED` after exhaustion |
| Temporary runtime failure | `RETRYABLE` |
| Retry exhaustion | `VALIDATION_REQUIRED` |
| Duplicate Agent Request | Idempotent handling; no duplicate accepted result |
| Same `agentRunId` conflicting result | `BLOCKED` |
| SHAP calculation failure | `VALIDATION_REQUIRED` |
| Contract validation failure | `VALIDATION_REQUIRED` |
| Audit event publish failure | `RETRYABLE` through outbox retry; `BLOCKED` only when durable publication cannot be guaranteed after retry policy exhaustion |
| Local LLM Runtime not running | Normal Phase 2 condition; must not fail E2E |

Exact codes and route mapping are owned by `rippleguard-contracts` and `rippleguard-governance-service`.
