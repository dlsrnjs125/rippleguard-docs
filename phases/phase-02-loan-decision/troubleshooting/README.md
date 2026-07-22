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

현상: `rippleguard-infra`의 `phase2-e2e.sh`가 실제 서비스를 호출하지 않고 `BLOCKED`를 반환한다.

영향 Repository: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-agent-runtime`, `rippleguard-infra`, `rippleguard-audit-replay-service`

Phase Gate 영향: Phase 2 `VERIFIED` 금지, Phase 3·4 handoff 금지

재현 방법: `../rippleguard-infra/scripts/phase2-e2e.sh`

원인: Loan submitted event는 materialized Phase 2 feature payload를 제공하지 않고, Governance는 immutable reference를 전달하며, Agent Runtime은 executable feature payload를 요구한다.

검토한 대안: fixture-only payload injection, direct DB insert, synthetic Kafka final event, mock Agent container, Agent Runtime snapshot resolver

선택 또는 권장 해결책: Loan/Governance에 production feature snapshot provider를 추가하거나 Agent Runtime이 approved production data path로 immutable snapshot reference를 resolve하게 한다.

검증 기준: Loan Service -> Governance -> Agent Runtime -> Governance validation -> Audit Timeline full E2E PASS

현재 상태: `BLOCKED`

Follow-up Repository: `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-agent-runtime`, `rippleguard-infra`

### 2026-07-22 — Phase 2 image provenance incomplete

현상: `python3 scripts/verify-phase2-images.py` fails because Phase 2 images are not inspectable locally and infra manifest records `imageDigest: null`.

영향 Repository: `rippleguard-infra`, all Phase 2 service image repositories

Phase Gate 영향: Image provenance gate blocks `VERIFIED`

재현 방법: `cd ../rippleguard-infra && python3 scripts/verify-phase2-images.py`

원인: Service images are not published with immutable digests in the baseline; Agent Runtime Dockerfile also lacks OCI label args.

검토한 대안: commit tag only, optional digest skip, local image id as final evidence

선택 또는 권장 해결책: publish immutable images, pin registry digests, verify OCI revision/source labels.

검증 기준: `make phase2-preflight` and `make phase2-verify-images` PASS with digest-pinned images.

현재 상태: `BLOCKED`

Follow-up Repository: `rippleguard-agent-runtime`, `rippleguard-infra`, service repositories
