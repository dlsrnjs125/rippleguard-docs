# Phase 1 Troubleshooting

Phase 1 진행 중 발생한 재현 가능한 문제, 원인, 해결, 검증 결과를 연결한다.

| ID | 상태 | 문제 | 원인 | 영향 | 검증·대응 |
| --- | --- | --- | --- | --- | --- |
| P1-TS-001 | OPEN | Phase 1 service image provenance verification failed | Local commit-tagged images did not expose `org.opencontainers.image.revision` or `org.opencontainers.image.source` labels expected by `rippleguard-infra/manifests/phase1-core-msa.json` | Phase 1 cannot be promoted to `VERIFIED`; Docker Compose runtime checks were not recorded as PASS | `python3 scripts/verify-phase1-images.py manifests/phase1-core-msa.json` failed in `rippleguard-infra`; rebuild/retag images with required labels, then rerun image verification and Phase 1 runtime checks |
| P1-TS-002 | RESOLVED_FOR_LOCAL_RUN | Service tests initially failed in sandbox | Sandbox blocked Docker/Testcontainers socket access and Mockito Byte Buddy self-attach | Initial service test attempts could not be used as PASS evidence | Approved execution outside sandbox passed `./mvnw test` for Loan, Governance and Audit services |

새 항목은 [Troubleshooting Template](../../../templates/troubleshooting-template.md)을 사용한다.
