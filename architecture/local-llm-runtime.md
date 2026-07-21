# Local LLM Runtime

## Purpose

RippleGuard uses a Local LLM Runtime so unstructured specialist agents can run without making an external paid API a required MVP path. Model serving, Agent Runtime deployment, failure handling and version baselines are separated so a model change does not imply a service code change.

## MVP Runtime

```text
macOS Host
├─ Ollama
│  └─ Quantized Instruct Model
│
└─ Docker Compose
   ├─ Agent Runtime
   ├─ Governance Service
   ├─ Loan Service
   ├─ Audit Service
   ├─ Kafka
   └─ PostgreSQL
```

Initial operation uses one quantized instruct model shared by the RippleGuard Consequence Agent and Evidence & Control Agent. Agent independence is provided by purpose, input contract, prompt, tool allowlist, output schema, run ID, permission and evaluation dataset, not by requiring separate foundation models.

## Access Boundary

Allowed:

```text
Agent Runtime -> Ollama
```

Forbidden:

```text
Governance Service -> Ollama
Loan Service -> Ollama
Audit Service -> Ollama
Web -> Ollama
```

Governance Service validates structured outputs and chooses execution routes. Loan Service owns final loan state. Neither service calls the Local LLM Runtime directly.

## Provider Abstraction

```text
StructuredLlmPort
├─ OllamaAdapter
├─ LlamaCppAdapter
└─ ExternalApiAdapter
```

Agent Graph nodes must call `StructuredLlmPort`, not an Ollama-specific SDK. Ollama is the MVP default provider. llama.cpp is a later reproducibility or GGUF execution candidate. External API providers are optional benchmark or adapter candidates, not an MVP required path.

External API provider is not an automatic runtime fallback. Provider switching requires an explicitly versioned execution policy, approved data boundary, provider manifest, evaluation baseline and audit trace. An Ollama timeout must not automatically send applicant context or document content to an external provider.

## Execution Flow

```text
Agent Run start
-> Model Manifest validation
-> Context Budget validation
-> Local LLM call
-> Structured Output
-> Pydantic Validation
-> JSON Schema Validation
-> Deterministic Validator
-> Agent Output Event
```

Internal Chain-of-Thought is not stored, emitted to Kafka or shown in Graph views. Full prompts and full LLM responses are not published as events. Audit receives structured results, reason codes, references and version metadata.

## Structured Validation Roles

The three validation steps have separate responsibilities:

- Pydantic: Agent Runtime internal parsing and type validation.
- JSON Schema: cross-repo executable contract validation from `rippleguard-contracts`.
- Deterministic Validator: domain rules, allowed scope transitions and reference integrity.

Passing schema validation does not imply a result is safe. Governance can still reject a result that violates domain meaning, policy constraints or reference integrity.

## Context Budget

Context Budget is an Agent Runtime responsibility and must be recorded as versioned metadata. The baseline fields are:

- `maximumInputTokens`
- `reservedOutputTokens`
- `systemPromptTokens`
- `evidenceTokenBudget`
- `truncationPolicy`
- `chunkSelectionVersion`
- `overflowBehavior`
- `contextCompleteness`: `COMPLETE`, `PARTIAL_ALLOWED` or `INCOMPLETE_BLOCKING`
- `omittedEvidenceCount`
- `truncatedEvidenceCount`
- `requiredEvidenceMissing`

Trace metadata must record which evidence references were included, omitted or truncated. A context overflow must fail safely; it must not silently drop required evidence and continue as if the evaluation were complete.

If required evidence is missing or truncated, the result must not be marked as a normal completed evaluation. It must fail or route to Governance as `VERIFICATION_REQUIRED`. `PARTIAL_ALLOWED` is only valid when the omitted evidence is non-required and the output contract preserves the partial-context status.

## Failure Principles

The following failures are not safe results:

- Model Runtime unavailable
- Inference timeout
- Context overflow
- Malformed output
- Model digest mismatch

Failure path:

```text
Agent Evaluation Failed
-> Governance VERIFICATION_REQUIRED
-> automation reduction
```

The system must not substitute mock results, reuse a previous successful result, auto-approve, or bypass validation after a Local LLM failure.

Failure causes must be preserved even when Governance routes all of them to `VERIFICATION_REQUIRED`. Initial failure code candidates are:

- `MODEL_RUNTIME_UNAVAILABLE`
- `MODEL_LOAD_FAILED`
- `MODEL_DIGEST_MISMATCH`
- `INFERENCE_TIMEOUT`
- `CONTEXT_BUDGET_EXCEEDED`
- `MALFORMED_STRUCTURED_OUTPUT`
- `SCHEMA_VALIDATION_FAILED`
- `DETERMINISTIC_VALIDATION_FAILED`

Phase 3 must define Agent Failure Event Schema, failure code taxonomy, retryable/non-retryable classification, Governance route mapping and Audit storage contract before the OllamaAdapter failure drill is considered complete.

## Model Manifest Ownership

Model Manifest Schema and Model Manifest Instance are separate artifacts.

| Artifact | Owner | Responsibilities |
| --- | --- | --- |
| Model Manifest Schema | `rippleguard-contracts` | JSON Schema, required fields, version rules, compatibility policy, examples and invalid fixtures |
| Model Manifest Instance | `rippleguard-agent-runtime` | Actual model name, provider, quantization, digest fields, runtime version, context limit, license and source metadata |
| Published Model Baseline | `rippleguard-infra` | Commit-pinned manifest instance used by Docker Compose and runtime verification |

The manifest must not use one ambiguous `modelDigest` field. It must distinguish the digest target and algorithm.

Example fields:

```yaml
model:
  logicalName: rippleguard-specialist
  sourceProvider: huggingface
  sourceRepository: organization/model-name
  sourceRevision: exact-revision
  artifactType: GGUF
  artifactDigestAlgorithm: sha256
  artifactDigest: TBD
  quantization: Q4_K_M

runtime:
  provider: ollama
  providerVersion: TBD
  providerManifestDigest: TBD
  modelfileDigest: TBD
```

If a later adapter uses LoRA or multiple files, the manifest must either add explicit adapter digests or define a composite digest with deterministic construction rules. Phase 8 digest mismatch drills are meaningful only after Phase 3 fixes these digest semantics.

## Tabular Model Manifest

Loan Decision Agent uses a separate tabular model manifest. It can share common manifest envelope fields, but it must have `modelType: TABULAR` and tabular-specific provenance.

Example fields:

```yaml
modelType: TABULAR
framework: xgboost
frameworkVersion: TBD
artifactDigestAlgorithm: sha256
artifactDigest: TBD
featureSchemaVersion: TBD
preprocessingVersion: TBD
trainingDatasetRef: TBD
trainingCodeCommit: TBD
evaluationDatasetVersion: TBD
randomSeed: TBD
thresholdVersion: TBD
explainer:
  type: SHAP
  version: TBD
  configVersion: TBD
```

This manifest is required for Phase 2 reproducibility and is independent of Local LLM provider manifests.

## Model Weight Management

Tracked in GitHub:

- Model Manifest
- Modelfile or download script
- Artifact digest, provider manifest digest and Modelfile digest semantics
- Source repository and exact source revision
- License ID and license URL or license file reference
- Commercial use and redistribution flags
- Model card reference
- Download timestamp
- Prompt Version
- Evaluation result

Kept outside Git:

- Actual Model Binary Artifact
- Ollama Model Cache
- GGUF files

Model weights are not committed to repositories. A model whose source and digest cannot be checked cannot be used as a baseline.
