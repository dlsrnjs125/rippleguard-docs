# Phase 2 Troubleshooting

Do not record every command typo or IDE issue here. Use this file for cross-repo issues that affect Phase 2 scope, contracts, reproducibility or integration.

## Record Format

Each entry must include:

- 현상
- 영향 범위
- 재현 방법
- 원인
- 검토한 대안
- 선택한 해결책
- 트레이드오프
- 검증 방법
- 관련 PR·commit
- 후속 조치

## Recordable Issues

- Model reproducibility failure
- XGBoost and LightGBM result drift
- Feature ordering issue
- Serialization/deserialization issue
- SHAP explainer compatibility
- Python package version difference
- Model Binary Artifact digest mismatch
- Kafka duplicate event
- Timeout and retry duplicate execution
- Governance output validation failure
- Docker volume or MinIO artifact path issue
- ARM64 and AMD64 model binary difference
- Local development and container result mismatch

## Current Entries

### 2026-07-22 — Full Phase 2 E2E blocked by Snapshot/Feature Payload boundary

현상: `rippleguard-infra`의 `phase2-e2e.sh`가 실제 서비스를 호출하지 않고 `BLOCKED`를 반환했다.

영향 Repository: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-agent-runtime`, `rippleguard-infra`, `rippleguard-audit-replay-service`

Phase Gate 영향: Phase 2 `VERIFIED` 금지, Phase 3·4 handoff 금지

재현 방법: `../rippleguard-infra/scripts/phase2-e2e.sh`

원인: Loan submitted event는 materialized Phase 2 feature payload를 제공하지 않고, Governance는 immutable reference를 전달하며, Agent Runtime은 executable feature payload를 요구한다.

검토한 대안: fixture-only payload injection, direct DB insert, synthetic Kafka final event, mock Agent container, Agent Runtime snapshot resolver

선택한 해결책: Loan Service가 immutable Feature Snapshot API를 제공하고, Governance가 해당 API로 snapshot identity와 feature payload를 고정한 뒤 Agent Runtime에 전달한다.

검증 기준: Loan Service -> Governance -> Agent Runtime -> Governance validation -> Audit Timeline full E2E PASS

현재 상태: `RESOLVED_FOR_HAPPY_PATH`

검증 Evidence: `../rippleguard-infra/evidence/phase2/happy-path.json`

잔여 제한: 필수 failure drill 10개는 아직 real runtime injection으로 구현되지 않아 Phase 2 최종 Gate는 `BLOCKED`다.

Follow-up Repository: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-agent-runtime`, `rippleguard-infra`

### 2026-07-22 — Phase 2 image provenance incomplete

현상: `python3 scripts/verify-phase2-images.py` failed because Phase 2 images were not inspectable locally and infra manifest recorded `imageDigest: null`.

영향 Repository: `rippleguard-infra`, all Phase 2 service image repositories

Phase Gate 영향: Image provenance gate blocks `VERIFIED`

재현 방법: `cd ../rippleguard-infra && python3 scripts/verify-phase2-images.py`

원인: Service images are not published with immutable digests in the baseline; Agent Runtime Dockerfile also lacks OCI label args.

검토한 대안: commit tag only, optional digest skip, local image id as final evidence

선택한 해결책: build commit-tagged local images, record local image digests/image IDs in the infra baseline and verify OCI revision/source labels.

검증 기준: `make phase2-preflight` and `make phase2-verify-images` PASS with local compose baseline digests and OCI labels.

현재 상태: `RESOLVED_FOR_LOCAL_BASELINE`

Follow-up Repository: `rippleguard-agent-runtime`, `rippleguard-infra`, service repositories

잔여 제한: Pullable registry digest publication is productionization follow-up.

### 2026-07-24 — Snapshot timestamp precision mismatch

현상: Happy Path E2E failed with `SNAPSHOT_DIGEST_MISMATCH` even though the feature payload content was otherwise stable.

영향 Repository: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-infra`

Phase Gate 영향: Phase 2 Happy Path E2E and immutable snapshot identity verification failed.

재현 방법: `cd ../rippleguard-infra && make phase2-e2e`

원인: Loan DB persisted `created_at` at microsecond precision, while the snapshot reference carried a nanosecond timestamp. Governance correctly performed strict identity comparison and rejected the mismatch.

검토한 대안: Infra timestamp rewrite, Governance comparison relaxation, API-only comparison without DB confirmation.

선택한 해결책: Loan Service uses the DB-persisted timestamp value consistently in the snapshot row, snapshot reference and API response.

검증 방법: Infra Happy Path evidence compares DB `createdAt`, snapshot reference `snapshotCreatedAt` and API `createdAt`; all three match `2026-07-24T04:35:31.961738Z`.

현재 상태: `RESOLVED`

관련 Evidence: `../rippleguard-infra/evidence/phase2/happy-path.json`

### 2026-07-24 — Required failure drills not implemented with real runtime injection

현상: `make phase2-verify` fails ten required failure drill gates with exit code `2`.

영향 Repository: `rippleguard-infra` plus service-specific injection points in Loan, Governance, Agent Runtime and Audit.

Phase Gate 영향: Phase 2 `VERIFIED`, `PUBLISHED` manifest and Phase 3·4 verified handoff are blocked.

재현 방법: `cd ../rippleguard-infra && make phase2-verify`

원인: Retry, timeout, duplicate, conflict, artifact, contract, snapshot and recovery drills are recorded but do not yet execute real runtime injection paths.

검토한 대안: Marking blocked drill reports as PASS, using `curl || true`, direct DB manipulation, mock agent, fixed sleep readiness.

선택한 해결책: Keep Phase 2 `BLOCKED` until each drill has a real injection mechanism, bounded evidence and exit code `0`.

검증 방법: `make phase2-verify` must exit `0`; infra manifest must record `publicationStatus = PUBLISHED`, `verification.status = PASSED` and `knownBlockers = []`.

현재 상태: `BLOCKED`
