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

Service Dockerfiles build runnable images but do not embed the required OCI provenance labels. The Infra manifest expects commit-tagged images to prove that the image bytes were built from the recorded service repository and source commit.

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
3. Rebuild commit-tagged images from the recorded main commits.
4. Update Infra manifest if tags or digests change.
5. Rerun image verification and all Phase 1 runtime checks.
6. Publish sanitized summaries for Docs finalization.

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
