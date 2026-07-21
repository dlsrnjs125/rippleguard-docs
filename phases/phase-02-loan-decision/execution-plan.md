# Phase 2 Execution Plan

Phase 2 replaces the Phase 1 mock evaluation path with a reproducible Loan Proposal generated from a versioned financial Snapshot and a tabular model. This is a planning document; it does not implement schemas, services, models or Docker wiring.

## Current Status

Phase 2 remains `READY`. Phase 1 is published as `VERIFIED` on main, but Phase 2 implementation has not started. The next executable work is `rippleguard-contracts`.

## Repository Order

1. `rippleguard-docs`
   - Confirm scope, boundaries, work order, verification matrix and open questions.
2. `rippleguard-contracts`
   - Define Feature Schema, Versioned Snapshot references, Loan Proposal, Decision Envelope, Tabular Model Manifest, Agent Run metadata, request/result events, failure reason codes and fixtures.
3. `rippleguard-agent-runtime`
   - Prepare synthetic dataset, preprocessing, XGBoost/LightGBM comparison, model selection, binary artifact reference, manifest, `TabularModelPort`, SHAP and result handling.
4. `rippleguard-governance-service`
   - Request Agent execution for a fixed Snapshot and model version, validate output, connect Evaluation Run and Agent Run, and handle timeout/retry/failure states.
5. `rippleguard-audit-replay-service`
   - Record minimal Agent Run timeline and version metadata without pulling Phase 7 replay or hash-chain scope forward.
6. `rippleguard-infra`
   - Wire Agent Runtime image, model artifact reference, health checks and Phase 2 E2E validation.
7. `rippleguard-docs`
   - Record merged PRs, commits, images, evidence, baselines and final Phase 2 verification.

## Branch Plan

| Repository | Branch |
| --- | --- |
| `rippleguard-docs` | `docs/phase-2-loan-decision-kickoff` |
| `rippleguard-contracts` | `feat/phase-2-loan-decision-contracts` |
| `rippleguard-agent-runtime` | `feat/phase-2-loan-decision-agent` |
| `rippleguard-governance-service` | `feat/phase-2-agent-orchestration` |
| `rippleguard-audit-replay-service` | `feat/phase-2-agent-run-timeline` |
| `rippleguard-infra` | `feat/phase-2-agent-integration` |

Each branch is created from that repository's latest `main`. Branches are not shared across repositories.

## Pull Request Tracking Fields

Every Phase 2 PR must include:

- Phase: 2
- Parent planning document: this Phase 2 docs branch or its merged main commit
- Affected contracts
- Upstream dependency
- Downstream consumer
- Verification command
- Evidence path
- Known limitations
- Follow-up phase

## Scope Exclusions

Phase 2 excludes Local LLM inference, Ollama, RippleGuard Agent, Evidence Agent, Prompt design, external LLM APIs, Consequence/Evidence judgments, Web UI, final Loan state changes and Phase 3 `StructuredLlmPort` executable contract finalization.
