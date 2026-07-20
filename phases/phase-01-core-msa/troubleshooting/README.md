# Phase 1 Troubleshooting

Phase 1 진행 중 발생한 재현 가능한 문제, 원인, 해결, 검증 결과를 연결한다.

## TS-PH1-IMAGE-001: OCI Provenance Label Mismatch

Status: RESOLVED

### Symptom

Initial `rippleguard-infra` image verification failed because the expected OCI labels were missing from the local commit-tagged service images.

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

- Initial `rippleguard-infra` runtime verification was blocked.
- Docker Compose E2E, duplicate, recovery, outbox recovery and timeline checks were not recorded as PASS until provenance-valid images existed.
- Phase 1 and Phase 2 status transitions were blocked until remediation.

### Non-Solution

Do not:

- Weaken Infra verification.
- Manually relabel an unverified image.
- Change only the Infra manifest.
- Mark Runtime E2E PASS without provenance.

### Resolution

1. Loan PR #2 added OCI labels and merged at `e403c0a60ccb1cebf03380832d047f3fc01019e0`.
2. Governance PR #2 added OCI labels and merged at `45790ebd5de1c458f87a38b1a067b46c15a59134`.
3. Audit PR #2 added OCI labels and merged at `83ca52edda2f608f90d10694428dff6dffee8a23`.
4. Governance PR #3 stabilized event ordering and merged at `4e06e672affddc02d7e6662f3022d00de86bb3b9`.
5. Infra PR #3 rebuilt commit-tagged images from clean service checkouts, verified OCI labels and passed all runtime checks.
6. Docs PR #7 records Phase 1 `VERIFIED` and Phase 2 `READY`.

### Exit Criteria

- All three service images pass `verify-phase1-images.py`.
- `phase1-check`, `phase1-e2e`, duplicate, recovery, outbox recovery and order/timeline checks pass.
- Docs pins the new evidence and records Phase 1 as `VERIFIED`.

## TS-PH1-E2E-001: Runtime Verification Blocked by Image Baseline

Status: RESOLVED

Primary Owner:

- `rippleguard-infra`

Dependency:

- TS-PH1-IMAGE-001

Runtime commands are PASS after provenance-valid service images and Governance event ordering remediation.

## TS-PH1-DOC-001: Integration Matrix Capability Drift

Status: MITIGATED

Primary Owner:

- `rippleguard-docs`

Symptom:

- Central Docs overclaimed some producer/consumer paths compared with the Infra Phase 1 manifest.

Resolution:

- Split contract readiness from producer implementation, consumer implementation and runtime verification.
- Compare the Integration Matrix with the Infra service capability manifest before Phase status changes.

Current Result:

- Phase 1 core manifest paths are recorded as runtime verified.
- Deferred producer/consumer paths remain explicitly marked out of core runtime scope.

새 항목은 [Troubleshooting Template](../../../templates/troubleshooting-template.md)을 사용한다.
