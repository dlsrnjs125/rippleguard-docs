# Phase 2 Integration Plan

## Integration Shape

```text
Governance Service
-> immutable Loan Feature Snapshot lookup
-> fixed Snapshot, feature payload and model version request
-> Agent Runtime Loan Decision Agent
-> Loan Proposal / Decision Envelope
-> Governance validation
-> persisted request event causation chain
-> Governance validated/rejected Audit event
-> Audit minimal Agent Run timeline
```

Loan Service remains the only owner of final loan state. Loan Decision Agent produces proposals only.

## Required Integration Checks

- Governance requests a fixed Snapshot and model version.
- Agent Runtime returns schema-valid Loan Proposal or explicit failure.
- Governance does not adopt invalid output.
- Agent Runtime does not publish the same Phase 2 result directly to Audit Replay.
- Governance emits the Audit trace only after validation.
- Timeout and retry state is explicit.
- Mock success result is not used as fallback.
- Audit can retrieve Agent Run metadata.
- Infra E2E passes without Local LLM Runtime.
- Failure drills use real runtime injection, not synthetic PASS reports.

## Infra Scope

Infra owns Agent Runtime wiring, service health, environment variables, model artifact mount or MinIO reference, startup dependency and Phase 2 validation scripts.

Infra does not own model training, business logic, contract schema meaning or Phase 3 Local LLM runtime.

## Minimal E2E

The Phase 2 E2E must show:

1. Loan application submit enters Governance path.
2. Loan Service exposes immutable Feature Snapshot identity and feature payload.
3. Governance creates an Evaluation Run and persists the request event.
4. Agent Runtime generates a Loan Proposal from a fixed Snapshot/model version.
5. Governance validates the result and emits validation causation to the request event ID.
6. Audit exposes the request and validation timeline.
7. Final Loan state is not changed by Phase 2.
8. Local LLM Runtime is not required.

## Final Review Status

Current status: `BLOCKED`

The latest final review found that the minimal Happy Path E2E is executable from the current local main baselines. Infra evidence proves immutable Feature Snapshot acquisition, Governance snapshot validation, Agent Runtime inference, request-event causation, Audit timeline projection, final Loan state non-mutation, reproducibility and Local LLM absence.

Phase 2 remains blocked because the required failure drills are still not implemented with real runtime injection. The next integration work must implement retry, timeout, duplicate, conflict, artifact, contract, snapshot and recovery drills without direct DB business inserts, synthetic Kafka final events, mock Agent containers, `curl || true`, fixed-sleep readiness or Local LLM fallbacks.
