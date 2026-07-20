# Phase 1 Troubleshooting

Phase 1 진행 중 발생한 재현 가능한 문제, 원인, 해결, 검증 결과를 연결한다.

## TS-PH1-IMAGE-001: OCI Provenance Label Mismatch

Status: OPEN

### Symptom

`rippleguard-infra` image verification fails because the expected OCI labels are missing from the local commit-tagged service images.

Failed command:

```sh
python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json
```

Missing labels:

- `org.opencontainers.image.revision`
- `org.opencontainers.image.source`

### Affected Repositories

- `rippleguard-loan-service`
- `rippleguard-governance-service`
- `rippleguard-audit-replay-service`
- `rippleguard-infra`
- `rippleguard-docs`

### Root Cause

Service Dockerfiles build runnable images but do not embed the required OCI provenance labels. OCI labels establish the declared source repository and revision baseline expected by the Infra manifest. They do not cryptographically prove that the image contents were built from that commit; registry digest pinning and build attestations are deferred.

### Owner

Primary:

- `rippleguard-loan-service`
- `rippleguard-governance-service`
- `rippleguard-audit-replay-service`

Secondary:

- `rippleguard-infra` updates image baselines and reruns runtime verification after service images are rebuilt.
- `rippleguard-docs` updates the Phase 1 published baseline only after Infra PASS evidence exists.

### Impact

- `rippleguard-infra` runtime verification is blocked.
- Docker Compose E2E, duplicate, recovery, outbox recovery and timeline checks cannot be recorded as PASS.
- Phase 1 cannot be promoted to `VERIFIED`.
- Phase 2 cannot be promoted to `READY`.

### Non-Solution

Do not:

- Weaken Infra verification.
- Manually relabel an unverified image.
- Change only the Infra manifest.
- Mark Runtime E2E PASS without provenance.

### Resolution

1. Update each service Dockerfile/build workflow.
2. Merge service follow-up PRs.
3. Record each follow-up PR merge commit as the revised service baseline.
4. Build a commit-tagged image from that new merge commit.
5. Verify OCI revision/source labels against the new commit.
6. Update the Infra manifest and local tagging script.
7. Rerun image verification and all Phase 1 runtime checks.
8. Publish sanitized summaries for Docs finalization.

### Exit Criteria

- All three service images pass `verify-phase1-images.py`.
- `phase1-check`, `phase1-e2e`, duplicate, recovery, outbox recovery and order/timeline checks pass.
- Docs pins the new evidence and records Phase 1 as `VERIFIED`.

## TS-PH1-E2E-001: Runtime Verification Blocked by Image Baseline

Status: BLOCKED

Primary Owner:

- `rippleguard-infra`

Dependency:

- TS-PH1-IMAGE-001

Runtime commands are intentionally not marked PASS until provenance-valid service images are available.

## TS-PH1-DOC-001: Integration Matrix Capability Drift

Status: OPEN

Primary Owner:

- `rippleguard-docs`

Symptom:

- Central Docs overclaimed some producer/consumer paths compared with the Infra Phase 1 manifest.

Resolution:

- Split contract readiness from producer implementation, consumer implementation and runtime verification.
- Compare the Integration Matrix with the Infra service capability manifest before Phase status changes.

새 항목은 [Troubleshooting Template](../../../templates/troubleshooting-template.md)을 사용한다.
