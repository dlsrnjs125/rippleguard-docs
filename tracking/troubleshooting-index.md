# Troubleshooting Index

서비스 내부 문제의 상세 원인·시도·해결은 소유 Repository에 기록하고 중앙에는 영향과 링크만 남긴다. 여러 Repository의 계약 또는 통합 문제는 중앙 Phase 디렉터리에 별도 요약할 수 있다.

| ID | Phase | Problem | Primary Owner | Secondary Owner | Status | Resolution Link |
| --- | --- | --- | --- | --- | --- | --- |
| TS-PH1-IMAGE-001 | 1 | Missing OCI provenance labels | Loan, Governance and Audit service repositories | `rippleguard-infra`, `rippleguard-docs` | OPEN | [Phase 1 Troubleshooting](../phases/phase-01-core-msa/troubleshooting/README.md) |
| TS-PH1-E2E-001 | 1 | Runtime verification blocked by image baseline | `rippleguard-infra` | service image PRs | BLOCKED | [Phase 1 Troubleshooting](../phases/phase-01-core-msa/troubleshooting/README.md) |
| TS-PH1-DOC-001 | 1 | Integration matrix capability drift | `rippleguard-docs` | `rippleguard-infra` manifest | OPEN | [Phase 1 Troubleshooting](../phases/phase-01-core-msa/troubleshooting/README.md) |

상세 문서 링크는 실제 URL이나 확인된 Repository 경로만 기록한다. Cross-Repo 요약은 계약 변경, 영향 Consumer, 호환 Baseline과 관련 ADR을 포함하되 서비스 내부 로그를 복사하지 않는다.
