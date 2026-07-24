# Repository Map

Phase 0에서 합의한 주 언어와 핵심 프레임워크를 기록한다. 구체 버전은 구현 Repository에서 검증 후 확정한다.

| Repository | 역할 | 기술 | 소유 데이터 | 주요 의존성 |
| --- | --- | --- | --- | --- |
| `rippleguard-docs` | 전체 Architecture, Local LLM 전략, ADR, Roadmap, Phase Gate, Risk, Cross-Repo Baseline | Markdown | 문서 기준 | 모든 Repository의 검증 정보 |
| `rippleguard-contracts` | 실행 가능한 통신·Agent·정책 계약, 향후 Tabular/Local LLM Model Manifest Schema, Agent Run Metadata, Structured Output, Provider Metadata, Failure Reason Code | OpenAPI·JSON Schema 등 형식은 구현 시 확정 | Versioned Schema, Model Manifest JSON Schema, 필수 필드, 호환성 정책, 예제와 Invalid Fixture | 모든 Producer·Consumer |
| `rippleguard-infra` | 로컬 통합 플랫폼, Ollama 연결 설정, Model Runtime Health, Agent Runtime Environment, Published Model Manifest Baseline, 장애 Drill | Docker Compose, Kafka KRaft, Kafka UI, PostgreSQL, OPA, MinIO | 환경 설정, Kafka Topic, 로컬 Platform 기준, 통합 Baseline Manifest | Contracts, 각 서비스 Image |
| `rippleguard-loan-service` | 신청, Snapshot, 최종 대출 상태; Local LLM 직접 연동 없음 | Spring Boot, PostgreSQL, Kafka | `loan_db` | Contracts, Kafka |
| `rippleguard-governance-service` | Case, Evaluation Run, 채택 Finding Snapshot, Assurance, 정책·실행 통제, Execution Plan, Agent Output Validation, Timeout·Failure 상태 처리 | Spring Boot, PostgreSQL, Kafka, OPA | `governance_db` | Contracts, Kafka, OPA |
| `rippleguard-agent-runtime` | TabularModelPort, StructuredLlmPort, Ollama Adapter, llama.cpp Adapter, Agent Graph, Prompt, Tabular/Local LLM Model Manifest Instance 작성과 검증 | Python, FastAPI, LangGraph, pgvector | checkpoint/RAG 파생 데이터, tabular framework/dataset/feature/explainer metadata, local model name/provider/quantization/digest/runtime/context/license/source metadata | Contracts, Kafka, 허용 Tool, MinIO, Local LLM Runtime |
| `rippleguard-audit-replay-service` | Trace, Tamper-Evident Audit, Replay, Version Diff, Model·Prompt·Tool Version Metadata, Agent Run Timeline, Structured Result Reference | Spring Boot, PostgreSQL, Kafka | `audit_db` | Contracts, Kafka, MinIO |
| `rippleguard-web` | 신청자·심사자·Governance UI, Agent Run과 Model Metadata 표시, Chain-of-Thought 비표시 | React 또는 Next.js | 영속 업무 원장 없음 | 공개 REST API |

전체 기준은 [Technology Stack](../architecture/technology-stack.md)에 설명한다.

## Phase 2 Final Review Note

Phase 2 remains blocked at the cross-repository gate, but the blocker changed after remediation. `rippleguard-infra` now verifies local image provenance, immutable Loan Feature Snapshot acquisition, real Happy Path E2E, request-event causation, Audit timeline projection, reproducibility and Local LLM absence. Phase 2 cannot be promoted to `VERIFIED` because retry, timeout, duplicate, conflict, artifact, contract, snapshot and recovery drills still need real runtime injection evidence.
