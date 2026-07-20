# ADR-007: Local LLM First Agent Strategy

- Status: Accepted
- Date: 2026-07-20

## Context

External LLM APIs add cost and network dependency. RippleGuard still needs LLM inference for unstructured specialist agents that analyze semantic transformation, downstream impact, document mismatch, missing evidence and legacy control loss.

Loan Decision is better served by a reproducible tabular ML model. The personal-project MVP also needs local execution and offline demo capability.

## Decision

RippleGuard is a Hybrid Multi-Agent Governance Platform.

- Loan Decision Agent uses XGBoost or LightGBM with SHAP and does not use an LLM.
- RippleGuard Consequence Agent and Evidence & Control Agent use a Local LLM through Ollama by default.
- The initial RippleGuard and Evidence Agents may share one quantized instruct foundation model.
- Agent independence is guaranteed by purpose, input contract, prompt, tool allowlist, output contract and run separation, not by requiring separate foundation models.
- Agent Runtime owns `StructuredLlmPort` so providers are isolated behind an adapter boundary.
- External API LLMs and llama.cpp are later adapter or benchmark candidates, not an MVP required path.

Governance Service validates structured outputs and determines execution routes. Loan Service remains the only service that changes final loan state.

## Alternatives

### External API Only

This can improve initial model quality, but it makes the MVP dependent on network availability, API keys, external cost and provider-specific behavior.

### Hugging Face Transformers In-Process Loading

This can keep inference local, but it couples Agent Runtime image size, dependency management and model serving lifecycle too tightly for the MVP.

### Separate LLM per Agent

This may reduce common-mode failure, but it increases memory, latency and model governance work before the system has evidence that separate models are needed.

### Rule-Only Specialist Agents

This improves determinism, but it is too limited for semantic transformation, downstream impact and document inconsistency analysis.

## Consequences

Benefits:

- No required API cost for the MVP.
- Data stays local by default.
- Offline demo is possible.
- Model version and digest can be recorded.
- Provider replacement remains possible through `StructuredLlmPort`.

Costs:

- Local memory constraints.
- Inference latency.
- Output quality variance.
- Model runtime operation.
- Model supply-chain verification.

## Validation

- Phase 2 succeeds without Local LLM Runtime.
- Phase 3 generates Consequence Envelopes without external paid APIs.
- Phase 3 records model name, digest, quantization and runtime version.
- Phase 4 proves shared-model agents remain independent by prompt, input, tool and output boundaries.
- Phase 8 verifies runtime down, timeout, context overflow, malformed output and digest mismatch safe failures.
- Phase 9 measures local model quality and common-mode failure, with optional external API benchmark if available.

## Related Documents

- [Local LLM Runtime](../architecture/local-llm-runtime.md)
- [Technology Stack](../architecture/technology-stack.md)
- [Agent Governance](../architecture/agent-governance.md)
- [MVP Scope](../foundation/mvp-scope.md)
