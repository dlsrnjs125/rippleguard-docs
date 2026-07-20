# Troubleshooting Index

서비스 내부 문제의 상세 원인·시도·해결은 소유 Repository에 기록하고 중앙에는 영향과 링크만 남긴다. 여러 Repository의 계약 또는 통합 문제는 중앙 Phase 디렉터리에 별도 요약할 수 있다.

| ID | Phase | Repository | 문제 | Cross-Repo 영향 | 상태 | 상세 문서 |
| --- | --- | --- | --- | --- | --- | --- |
| - | 0 | `rippleguard-docs` | 현재 기록 없음 | 없음 | - | [Phase 0](../phases/phase-00-foundation/troubleshooting/README.md) |
| P1-TS-001 | 1 | `rippleguard-infra`, service repositories | Phase 1 service image OCI provenance label 검증 실패 | Phase 1 `VERIFIED` 전환과 Docker Compose runtime evidence 고정 차단 | OPEN | [Phase 1 Troubleshooting](../phases/phase-01-core-msa/troubleshooting/README.md) |
| P1-TS-002 | 1 | local verification environment | Sandbox에서 Testcontainers/Mockito agent 접근 실패 | 최초 로컬 서비스 테스트 결과를 PASS evidence로 사용할 수 없음 | RESOLVED_FOR_LOCAL_RUN | [Phase 1 Troubleshooting](../phases/phase-01-core-msa/troubleshooting/README.md) |

상세 문서 링크는 실제 URL이나 확인된 Repository 경로만 기록한다. Cross-Repo 요약은 계약 변경, 영향 Consumer, 호환 Baseline과 관련 ADR을 포함하되 서비스 내부 로그를 복사하지 않는다.
