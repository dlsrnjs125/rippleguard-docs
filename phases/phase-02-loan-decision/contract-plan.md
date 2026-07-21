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
| Feature Schema Version mismatch | `VALIDATION_REQUIRED` or `BLOCKED` |
| Required Feature missing | `VALIDATION_REQUIRED` |
| Unsupported extra Feature | `VALIDATION_REQUIRED` |
| Snapshot lookup failure | `RETRYABLE` or `BLOCKED` |
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
| Audit event publish failure | `RETRYABLE` or `BLOCKED` |
| Local LLM Runtime not running | Normal Phase 2 condition; must not fail E2E |

Exact codes and route mapping are owned by `rippleguard-contracts` and `rippleguard-governance-service`.
