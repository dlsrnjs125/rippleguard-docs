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

## Model Weight Management

Tracked in GitHub:

- Model Manifest
- Modelfile or download script
- Digest
- Prompt Version
- Evaluation result

Kept outside Git:

- Actual Model Weight
- Ollama Model Cache
- GGUF files

Model weights are not committed to repositories. A model whose source and digest cannot be checked cannot be used as a baseline.
