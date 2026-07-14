# Repository Strategy

RippleGuard는 개인 GitHub 계정 아래 8개 독립 Repository로 운영하는 멀티레포 MSA다. 각 Repository는 책임 범위 안에서 독립적으로 빌드·테스트하고 불변 Docker Image를 만든다.

| Repository | 책임 |
| --- | --- |
| `rippleguard-docs` | 전체 기획, 아키텍처, ADR, Phase 추적, 통합 기준 |
| `rippleguard-contracts` | REST API, Kafka Event, Agent Output, Policy Input 계약 |
| `rippleguard-infra` | Docker Compose, Kafka, PostgreSQL, OPA, MinIO, 관측 환경과 통합 검증 |
| `rippleguard-loan-service` | 대출 신청, 금융 Snapshot, 최종 대출 상태 |
| `rippleguard-governance-service` | Decision Case, Assurance, Agent 상태, 정책과 실행 통제 |
| `rippleguard-agent-runtime` | Loan Decision, RippleGuard, Evidence & Control Agent 실행 |
| `rippleguard-audit-replay-service` | 실행 Trace, 감사로그, Replay, Version Diff |
| `rippleguard-web` | 신청자, 심사 담당자, Governance Console 화면 |

Repository 사이에 Entity, Repository, 내부 구현 코드를 공유하지 않는다. 통신은 `rippleguard-contracts`가 소유한 명시적 API·Event 계약을 사용하며, 전체 조합의 실행과 통합 검증은 `rippleguard-infra`에서 수행한다. 각 Phase에서 함께 검증된 Branch, Commit SHA, Docker Image, 계약 버전과 Migration 조합은 `rippleguard-docs`의 Cross-Repo Baseline에 고정한다.
