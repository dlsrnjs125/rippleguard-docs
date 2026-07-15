# Repository Map

Phase 0에서 합의한 주 언어와 핵심 프레임워크를 기록한다. 구체 버전은 구현 Repository에서 검증 후 확정한다.

| Repository | 역할 | 기술 | 소유 데이터 | 주요 의존성 |
| --- | --- | --- | --- | --- |
| `rippleguard-docs` | 전체 Architecture, ADR, Roadmap, Phase와 Baseline | Markdown | 문서 기준 | 모든 Repository의 검증 정보 |
| `rippleguard-contracts` | 실행 가능한 통신·Agent·정책 계약 | OpenAPI·JSON Schema 등 형식은 구현 시 확정 | Versioned Schema | 모든 Producer·Consumer |
| `rippleguard-infra` | 로컬 통합·배포·관측 | Docker Compose, Kafka, PostgreSQL, OPA, MinIO, OpenTelemetry, Prometheus, Grafana | 환경 설정 | 각 서비스 Image |
| `rippleguard-loan-service` | 신청, Snapshot, 최종 대출 상태 | Spring Boot, PostgreSQL, Kafka | `loan_db` | Contracts, Kafka |
| `rippleguard-governance-service` | Case, Assurance, 정책·실행 통제 | Spring Boot, PostgreSQL, Kafka, OPA | `governance_db` | Contracts, Kafka, OPA |
| `rippleguard-agent-runtime` | 세 종류 Agent 실행 | Python, FastAPI, LangGraph, pgvector | checkpoint/RAG 파생 데이터 | Contracts, Kafka, 허용 Tool, MinIO |
| `rippleguard-audit-replay-service` | Trace, Tamper-Evident Audit, Replay, Version Diff | Spring Boot, PostgreSQL, Kafka | `audit_db` | Contracts, Kafka, MinIO |
| `rippleguard-web` | 신청자·심사자·Governance UI | React 또는 Next.js | 영속 업무 원장 없음 | 공개 REST API |

전체 기준은 [Technology Stack](../architecture/technology-stack.md)에 설명한다.
