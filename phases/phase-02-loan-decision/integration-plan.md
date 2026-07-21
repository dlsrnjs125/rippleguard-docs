# Phase 2 Integration Plan

## Integration Shape

```text
Governance Service
-> fixed Snapshot and model version request
-> Agent Runtime Loan Decision Agent
-> Loan Proposal / Decision Envelope
-> Governance validation
-> Audit minimal Agent Run timeline
```

Loan Service remains the only owner of final loan state. Loan Decision Agent produces proposals only.

## Required Integration Checks

- Governance requests a fixed Snapshot and model version.
- Agent Runtime returns schema-valid Loan Proposal or explicit failure.
- Governance does not adopt invalid output.
- Timeout and retry state is explicit.
- Mock success result is not used as fallback.
- Audit can retrieve Agent Run metadata.
- Infra E2E passes without Local LLM Runtime.

## Infra Scope

Infra owns Agent Runtime wiring, service health, environment variables, model artifact mount or MinIO reference, startup dependency and Phase 2 validation scripts.

Infra does not own model training, business logic, contract schema meaning or Phase 3 Local LLM runtime.

## Minimal E2E

The Phase 2 E2E must show:

1. Loan application or test Snapshot enters Governance path.
2. Governance creates or references an Evaluation Run.
3. Agent Runtime generates a Loan Proposal from a fixed Snapshot/model version.
4. Governance validates the result.
5. Audit exposes minimal Agent Run metadata.
6. Local LLM Runtime is not required.
