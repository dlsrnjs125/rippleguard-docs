# Repository Map

기술 스택은 구현 Repository에서 확정되기 전까지 추측하지 않는다.

| Repository | 역할 | 기술 | 소유 데이터 | 주요 의존성 |
| --- | --- | --- | --- | --- |
| `rippleguard-docs` | 전체 문서, ADR, Phase와 Baseline | Markdown | 문서 기준 | 모든 Repository의 검증 정보 |
| `rippleguard-contracts` | 실행 가능한 통신·Agent·정책 계약 | TBD | Versioned Schema | 모든 Producer·Consumer |
| `rippleguard-infra` | 로컬 통합·배포·관측 | Docker Compose 외 TBD | 환경 설정 | 각 서비스 Image, Kafka, DB, OPA, MinIO |
| `rippleguard-loan-service` | 신청, Snapshot, 최종 대출 상태 | TBD | `loan_db` | Contracts, Kafka |
| `rippleguard-governance-service` | Case, Assurance, 정책·실행 통제 | TBD | `governance_db` | Contracts, Kafka, OPA |
| `rippleguard-agent-runtime` | 세 종류 Agent 실행 | TBD | checkpoint/RAG 파생 데이터 | Contracts, Kafka, 허용 Tool, MinIO |
| `rippleguard-audit-replay-service` | Trace, 감사, Replay, Version Diff | TBD | `audit_db` | Contracts, Kafka, MinIO |
| `rippleguard-web` | 신청자·심사자·Governance UI | TBD | 영속 업무 원장 없음 | 공개 REST API |
