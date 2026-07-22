# Phase 2 Final Cross-Repository Verification

Date: 2026-07-22

## 1. Phase 2 Final Verdict

`BLOCKED`

Phase 2 is not `VERIFIED`. Contracts and selected service-level implementation evidence exist, but the published cross-repository baseline does not satisfy the release gate because full production E2E, image digest provenance, and the Loan-to-Agent materialized feature path are still blocked.

## 2. Verdict Basis

Directly executed during this review:

| Command | Repository | Result | Evidence |
| --- | --- | --- | --- |
| `PYTHONDONTWRITEBYTECODE=1 make validate` | `rippleguard-contracts` | PASS | Validated 40 schemas, 1 OpenAPI document, 67 valid examples, 114 intentional invalid examples, 22 scenarios, and 16 fixture-backed compatibility checks |
| `PYTHONDONTWRITEBYTECODE=1 PYTEST_ADDOPTS='-p no:cacheprovider' make reproducibility-test` | `rippleguard-agent-runtime` | PASS | `tests/integration/test_reproducibility.py`: 1 passed |
| `make validate-static` | `rippleguard-infra` | PASS | Static infra validation passed |
| `make phase2-scaffold-check` with absolute sibling repo env vars | `rippleguard-infra` | PASS | Manifest, source checkout, artifact digest, compose mount source checks passed |
| `python3 scripts/verify-phase2-images.py` | `rippleguard-infra` | FAIL | All Phase 2 service images were not found locally; manifest image digests are also null |
| `./scripts/phase2-e2e.sh` | `rippleguard-infra` | BLOCKED | Script exits with blocker: Loan event lacks materialized Phase 2 feature payload required by Agent Runtime |

Commands intentionally not executed:

- Maven test/package commands in Loan, Governance, and Audit repositories were not run because they can update `target/` under read-only verification repositories.
- Docker image build commands were not run because image build outputs must not be promoted into baselines during docs verification.

## 3. Published Commit Baseline

| Repository | Published `origin/main` commit | Latest main summary |
| --- | --- | --- |
| `rippleguard-docs` | `6db708b1ff6f77c358103230b4bd25eb7e5479f9` | `docs: start phase 2 loan decision planning (#9)` |
| `rippleguard-contracts` | `f4012e8b5a0dcd5605b61652a5c39deacb14454b` | `feat: define phase 2 loan decision contracts (#4)` |
| `rippleguard-loan-service` | `e403c0a60ccb1cebf03380832d047f3fc01019e0` | `fix: add phase 1 image provenance labels (#2)` |
| `rippleguard-agent-runtime` | `35121627550e5999a084c123b610f47884aa01f7` | `feat: implement phase 2 loan decision agent (#1)` |
| `rippleguard-governance-service` | `6e5dee34a01429ac017774fcd7a238e2f1415481` | `feat: orchestrate phase 2 loan decision agent (#4)` |
| `rippleguard-audit-replay-service` | `f3162d3bf3ea2bcfd8d40c929607a84d88054082` | `feat: add phase 2 agent run timeline (#3)` |
| `rippleguard-infra` | `b4051c271fe3b82cb8b6d2b8b89517f98017165c` | `feat: add phase 2 agent integration infra (#4)` |
| `rippleguard-web` | `2f835e5fb65d9470f3a37b190ef14dd6d95a0eb0` | Not in Phase 2 gate |

## 4. Repository Verdicts

| Repository | Phase 2 responsibility | Observed implementation | Observed tests/evidence | Contract alignment | Integration alignment | Verdict | Blockers / follow-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `rippleguard-contracts` | Executable Phase 2 schemas, fixtures, validator | Feature schema/payload, snapshot reference, request/result, model manifest, governance validation event, failure fixtures present | Direct `make validate` PASS | Mostly aligned; contract doc still says validation `causationId` is nearest persisted event while runtime treats it as `agentRunId` | Provides fixtures but not runtime E2E | `PASS_WITH_LIMITATIONS` | Resolve causation policy mismatch |
| `rippleguard-loan-service` | Snapshot source and final loan state owner | Versioned `financial_snapshot` exists; submitted event carries `inputSnapshotVersion` | Maven tests observed in source, not executed in this review | Phase 1 event compatible with existing contracts | Does not publish materialized Phase 2 feature payload or immutable feature snapshot reference | `BLOCKED` | Add production Phase 2 feature/snapshot provider path |
| `rippleguard-agent-runtime` | Tabular inference, SHAP, model/artifact verification | XGBoost baseline, digest checks, feature validation, reproducibility test present | Direct reproducibility test PASS | Uses contracts and model manifest | Requires materialized feature payload; cannot consume current Loan/Governance immutable reference path | `PASS_WITH_LIMITATIONS` | OCI image labels/digest and cross-service snapshot resolution remain missing |
| `rippleguard-governance-service` | Agent orchestration, validation, retry/timeout state, outbox event | Phase 2 Agent Runtime client, identity fields, validation event, no proposal-to-command path documented and tested in source | Tests observed, not executed in this review | Uses contracts for request/result/event validation | Current integration sends immutable reference while Agent Runtime requires feature payload | `BLOCKED` | Connect production feature payload/snapshot resolution without mock fallback |
| `rippleguard-audit-replay-service` | Agent Run read model and timeline | Consumes `governance.agent-result.validated.v1`, quarantine, payload conflict recovery, Agent Run APIs | Tests observed, not executed in this review | Supports current governance event and rejects Agent Runtime producer | Cannot prove full Phase 2 timeline until E2E exists | `PASS_WITH_LIMITATIONS` | Reverify after E2E; keep metadata nullable unless event carries it |
| `rippleguard-infra` | Cross-repo integration gate | Phase 2 compose, manifest, scaffold check, blocked E2E script | Direct static/scaffold PASS; image verify FAIL; E2E BLOCKED | Manifest pins contract/model baselines | Full E2E intentionally not executable | `BLOCKED` | Publish digest-pinned images and implement real E2E |
| `rippleguard-docs` | Phase status, final review, risk and handoff gate | This review updates status to blocked | This document | N/A | N/A | `PASS_WITH_LIMITATIONS` | Keep Phase 3/4 handoff blocked |

## 5. Contracts Baseline

- Commit: `f4012e8b5a0dcd5605b61652a5c39deacb14454b`
- Direct verification: `make validate` PASS
- Coverage observed: valid and invalid fixtures for Phase 2 request/result, duplicate request/result, retry, conflicting result, model artifact mismatch, snapshot mismatch, audit digest/reference mismatch, broken causation, and Local LLM absent scenarios.
- Limitation: causation policy is inconsistent across contract docs and runtime compatibility. Contracts describe nearest persisted event cause; Governance/Audit currently use `causationId = agentRunId` for the validation event.

## 6. Service Image Baseline

| Service | Image tag in infra manifest | Image digest | OCI revision/source |
| --- | --- | --- | --- |
| Loan | `rippleguard-loan-service:e403c0a60ccb` | Missing evidence — blocks VERIFIED | Expected in manifest |
| Governance | `rippleguard-governance-service:6e5dee34a014` | Missing evidence — blocks VERIFIED | Expected in manifest |
| Agent Runtime | `rippleguard-agent-runtime:35121627550e` | Missing evidence — blocks VERIFIED | Dockerfile lacks OCI label wiring |
| Audit | `rippleguard-audit-replay-service:f3162d3bf3ea` | Missing evidence — blocks VERIFIED | Expected in manifest |

`python3 scripts/verify-phase2-images.py` failed in this review because the local images were not inspectable. Infra also records `imageDigest: null` for every Phase 2 service.

## 7. Model Artifact Baseline

| Field | Value |
| --- | --- |
| Model manifest | `../rippleguard-agent-runtime/artifacts/manifests/phase2-loan-xgboost.v1.0.0.json` |
| Model manifest digest | `sha256:feae355ac937e43be18069d6390936d1c4b8630deea92333d6e6de15c514b9e7` |
| Model artifact | `../rippleguard-agent-runtime/artifacts/models/phase2-loan-xgboost.v1.0.0.json` |
| Model artifact digest | `sha256:1780b376723b52ad04630a474c6bd2eeddab2e89caa13956e76b356595ed79df` |
| Threshold manifest digest | `sha256:c3a7e45bd4c65e57d3fb935e08213b16d144a4674365c16c0eedfce7c6b4e4ea` |
| Feature schema version | `phase-2-loan-features.v1.0.0` |
| Preprocessing version | `preprocess.v1.0.0` |
| Threshold version | `threshold.v1.0.0` |
| Random seed | `42` |
| Runtime image digest in model manifest | Placeholder `sha256:ffffffff...` — not release evidence |

## 8. Final Gate Matrix

| Verification item | Owner | Baseline | Method | Actual result | Evidence | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Contracts | contracts | `f4012e8` | `make validate` | PASS | Direct command output | PASS |
| Reproducibility | agent-runtime | `3512162` | `make reproducibility-test` | PASS for one test | Direct command output | PASS_WITH_LIMITATIONS |
| SHAP | agent-runtime | `3512162` | Source/test inspection | Test coverage observed, not executed directly | `tests/contract`, `tests/integration` | UNVERIFIED |
| Governance validation | governance | `6e5dee3` | Source/test inspection | Implementation and tests observed, not executed directly | source tests | UNVERIFIED |
| Retry/timeout | governance/runtime | `6e5dee3`, `3512162` | Source/test inspection | Test code observed, no cross-repo drill | source tests | UNVERIFIED |
| Audit Timeline | audit | `f3162d3` | Source/test inspection | Agent Run projection tests observed, no full E2E API result | source tests | UNVERIFIED |
| Image provenance | infra | `b4051c2` | `verify-phase2-images.py` | FAIL | Missing images and null digests | BLOCKED |
| Full E2E | infra | `b4051c2` | `phase2-e2e.sh` | BLOCKED | Direct command output | BLOCKED |
| Local LLM absent | infra/contracts | `b4051c2`, `f4012e8` | Contract fixture and infra docs/source | No Phase 2 LLM runtime dependency observed; no full E2E PASS | source/evidence | PASS_WITH_LIMITATIONS |
| Artifact failure | infra/runtime | model digest | Source/test inspection | Artifact digest checks observed; no integrated drill | source tests | UNVERIFIED |

## 9. Anti-Pattern, Hardcoding, Fallback, and Final State Findings

- Local LLM/Ollama is not required in Phase 2 runtime wiring.
- No production Phase 2 path was found that emits `loan.decision.commanded.v1` from Agent proposals; Loan final state remains Loan-owned.
- Phase 1 mock classes remain in Governance for historical Phase 1 behavior. Source and docs state Phase 2 does not fall back to mock evaluation, but full E2E is blocked so this is not end-to-end proven.
- Agent Runtime has no fallback model path in docs/source; LightGBM is documented as offline comparison only.
- Hardcoded version strings exist in manifests, compose env, tests, and code constants. They are acceptable as pinned baseline/config values, but must be reconciled through the infra manifest before release.
- Runtime image digest and service image digests are missing. This is a HIGH blocker.

## 10. Critical and High Blockers

| Severity | Blocker | Evidence | Phase gate impact |
| --- | --- | --- | --- |
| CRITICAL | Full production Phase 2 E2E is not executable | `phase2-e2e.sh` exits BLOCKED | Blocks `VERIFIED` and Phase 3/4 handoff |
| HIGH | Loan/Governance do not provide a materialized Phase 2 feature payload path compatible with Agent Runtime | Infra evidence and runtime requirement | Blocks actual Loan -> Governance -> Agent Runtime -> Audit path |
| HIGH | Service image digests are missing | Infra manifest has `imageDigest: null`; image verification failed | Blocks provenance gate |
| HIGH | Agent Runtime Dockerfile lacks OCI source/revision labels | Dockerfile inspection and infra evidence | Blocks image provenance |
| HIGH | Integrated failure drills are not executed | No full E2E; drills only exist as source tests/fixtures | Blocks release gate |
| MEDIUM | Governance/Audit validation-event causation policy diverges from contracts docs | Contracts docs vs Audit compatibility code | Must be fixed before final baseline |

## 11. Phase 3 and Phase 4 Handoff

Phase 3 and Phase 4 handoff is not approved. The verified Decision Envelope, production snapshot path, model provenance, Agent Run identity, governance validation outcome, and Audit Timeline are not jointly proven by a full Phase 2 E2E.

## 12. Follow-Up Repository Work

### Follow-up Repository: `rippleguard-loan-service`

Severity: HIGH

Observed commit: `e403c0a60ccb1cebf03380832d047f3fc01019e0`

Problem: Loan submitted events provide `inputSnapshotVersion`, but do not provide the materialized Phase 2 feature payload or immutable feature snapshot path required by Agent Runtime.

Required change: Add a production Phase 2 feature/snapshot provider path consumable by Governance/Agent Runtime.

Must not use: direct DB injection, fixture-only payload, final event injection, mock fallback.

Expected branch: `feat/phase-2-feature-snapshot-provider`

Required verification: real Loan submit -> Governance request -> Agent Runtime inference -> Governance validation -> Audit timeline E2E.

Phase Gate impact: Blocks `VERIFIED`.

### Follow-up Repository: `rippleguard-governance-service`

Severity: HIGH

Observed commit: `6e5dee34a01429ac017774fcd7a238e2f1415481`

Problem: Governance produces an immutable reference while Agent Runtime requires materialized feature input.

Required change: Resolve feature payload acquisition through an approved production path and keep timeout/retry/conflict handling.

Must not use: Phase 1 mock evaluator, previous proposal, default model, current application state fallback.

Expected branch: `feat/phase-2-feature-snapshot-integration`

Required verification: retry, timeout, conflict, invalid result, and successful validation with actual Agent Runtime.

Phase Gate impact: Blocks full E2E.

### Follow-up Repository: `rippleguard-agent-runtime`

Severity: HIGH

Observed commit: `35121627550e5999a084c123b610f47884aa01f7`

Problem: Docker image provenance is incomplete; Dockerfile does not emit required OCI labels and runtime image digest is placeholder.

Required change: Add OCI label args, publish immutable image digest, replace placeholder runtime image digest with release evidence.

Must not use: `latest`, mutable local tag as final evidence, dummy/fallback model.

Expected branch: `fix/phase-2-image-provenance`

Required verification: image inspect, digest pinning, artifact digest verification, reproducibility test.

Phase Gate impact: Blocks provenance gate.

### Follow-up Repository: `rippleguard-infra`

Severity: HIGH

Observed commit: `b4051c271fe3b82cb8b6d2b8b89517f98017165c`

Problem: Full Phase 2 E2E is intentionally blocked; service image digests are null.

Required change: After upstream feature path and image publishing, replace blocked E2E with real flow and pin image digests.

Must not use: synthetic Kafka final event, direct DB insert, mock Agent container, local LLM fallback.

Expected branch: `feat/phase-2-runtime-e2e`

Required verification: `phase2-preflight`, `phase2-verify`, failure drills, local LLM absent check.

Phase Gate impact: Blocks `VERIFIED`.

### Follow-up Repository: `rippleguard-contracts`

Severity: MEDIUM

Observed commit: `f4012e8b5a0dcd5605b61652a5c39deacb14454b`

Problem: Phase 2 validation event causation policy is ambiguous across docs and runtime.

Required change: Define whether `governance.agent-result.validated.v1.causationId` is nearest event id or `agentRunId`, then update schema docs/fixtures and consumers consistently.

Must not use: consumer-specific hidden exception without contract text.

Expected branch: `fix/phase-2-validation-causation-policy`

Required verification: contracts `make validate`, Governance event test, Audit causation/quarantine tests.

Phase Gate impact: Blocks final contract alignment.
