# Phase 2 Repository Workplan

## Summary

Phase 2 is Contract-First. `rippleguard-contracts` must define executable schemas and fixtures before consumers implement against them.

2026-07-22 final cross-repository verification supersedes this planning workplan where implementation status differs. Current gate status is `BLOCKED`; see [Final Cross-Repository Verification](final-review.md).

| Repository | Responsibility | Input Contract | Implementation Scope | Excluded Scope | Expected Files or Modules | Tests | Failure Paths | Outputs | Done When | Downstream Consumer | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-docs` | Phase 2 scope, work order, verification and cross-repo tracking | Published Phase 1 baseline | Planning docs, matrices, open questions | Service code, schemas, Docker | `phases/phase-02-loan-decision/*`, `tracking/*` | Markdown checks | Planning ambiguity | Planning PR | Docs merged | All Phase 2 repos | PR, commit, docs diff |
| `rippleguard-contracts` | Executable contracts and fixtures | Phase 1 contracts, Phase 2 docs | Feature Schema, Snapshot refs, Loan Proposal, Decision Envelope, Tabular Model Manifest, Agent Run metadata, events, failure codes | Producer implementation | Expected: `schemas/`, `events/`, `fixtures/`, docs | Schema, examples, invalid fixtures, compatibility | Version mismatch, invalid fixture, incompatible change | Schemas and fixtures | Valid pass, invalid reject, compatibility documented | Agent Runtime, Governance, Audit, Infra | PR, commit, validation log |
| `rippleguard-loan-service` | Versioned Snapshot impact review | Phase 2 Feature Schema and Snapshot Reference contract | Confirm existing Snapshot fields, immutability, digest/provenance availability and no direct Phase 2 final-state mutation | Model execution, Agent orchestration | Existing Snapshot, event or API modules if contracts require changes | Compatibility and regression tests if changed | Missing field, mutable Snapshot, digest mismatch | Impact review or narrow compatibility PR | Current Snapshot path is accepted or a service PR is merged | Agent Runtime, Governance | Review note, PR if needed, test output |
| `rippleguard-agent-runtime` | Loan Decision Agent execution | Contracts PR merged | Synthetic dataset, preprocessing, XGBoost/LightGBM comparison, model choice, model binary reference, manifest, `TabularModelPort`, SHAP, timeout/retry/schema failure handling, Docker image | Local LLM, RippleGuard/Evidence Agents, final Loan status | Expected: agent runtime app modules, model pipeline, tests, Dockerfile | Unit, reproducibility, contract, image build | Missing feature, model missing, digest mismatch, timeout, retry exhaustion | Agent image, manifest, report | Same input reproducible within tolerance; schema compliant output | Governance, Audit, Infra | PR, commit, report, image tag/digest |
| `rippleguard-governance-service` | Orchestration and validation | Contracts and Agent output contract | Request fixed Snapshot/model version, connect `agentRunId`, validate output, manage timeout/retry/failure states | Model training, final Loan state mutation, mock success substitution | Expected: service orchestration, validators, state handling tests | Unit, integration, contract | Timeout, retry exhaustion, invalid output, missing model | Evaluation/Agent Run linkage | Invalid output not adopted; failure state explicit | Audit, Infra | PR, commit, test output |
| `rippleguard-audit-replay-service` | Minimal Agent Run timeline | Contracts and Governance events | Model version, feature schema, preprocessing version, dataset ref, artifact digest, SHAP version, request/result correlation, failure timeline | Phase 7 full replay, hash chain, execution graph | Expected: timeline read model, event handlers, API response tests | Unit, contract, API/DB | Missing event, duplicate request, conflicting result | Timeline metadata query | Agent Run metadata retrievable | Infra, Docs | PR, commit, response sample |
| `rippleguard-infra` | Local integration | Service images and manifests | Agent Runtime image wiring, model artifact mount or MinIO ref, env vars, health checks, Phase 2 E2E scripts | Local LLM runtime, GHCR release, SBOM/SLSA unless separately planned | Expected: compose overlay, manifests, scripts, Make targets | Compose E2E, timeout/retry/schema/missing model/replay checks | Startup dependency, missing artifact, digest mismatch | Integration evidence | Phase 2 E2E passes without Local LLM Runtime | Docs | PR, commit, image tag/digest, report |

## Failure Classification

Each repository maps failures to one of:

- `RETRYABLE`
- `NON_RETRYABLE`
- `VALIDATION_REQUIRED`
- `BLOCKED`

Exact enum names are owned by `rippleguard-contracts`.
