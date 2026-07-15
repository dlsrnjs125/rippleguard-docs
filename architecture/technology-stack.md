# Technology Stack

Phase 0에서 언어와 핵심 프레임워크 경계를 고정한다. 구체적인 런타임·라이브러리 버전과 빌드 플러그인은 각 구현 Repository에서 검증 후 확정한다.

| 영역 | 선택 | 적용 범위 |
| --- | --- | --- |
| Loan, Governance, Audit Backend | Spring Boot | REST, Kafka Consumer/Producer, 서비스별 업무 로직 |
| Agent Runtime | Python, FastAPI, LangGraph | Agent 실행 API와 상태 그래프 |
| Loan Model | XGBoost 또는 LightGBM | 합성 데이터 기반 대출 제안 모델; Golden Case 비교 후 하나를 확정 |
| XAI | SHAP | 모델 입력 기여도 설명; Governance 판단을 대체하지 않음 |
| Event | Kafka | 서비스 간 상태 변경과 비동기 업무 |
| Policy | OPA | Versioned Assurance Routing 정책 |
| Database | PostgreSQL | 서비스별 독립 DB |
| RAG·Vector | pgvector | Agent Runtime의 목적 제한 검색 파생 데이터 |
| Document Object | MinIO | 원문 문서와 Evidence 객체 |
| Observability | OpenTelemetry, Prometheus, Grafana | 분산 Trace, Metric, Dashboard |
| Local Integration | Docker Compose | 모든 Image와 플랫폼 구성 통합 |
| FDS Simulator | 경량 Mock Server 또는 WireMock | 운영형 Demo REST Source; 고정 JSON Fixture는 Test 전용 |
| Web | React 또는 Next.js | 신청·심사·Governance Console; Phase 6 전에 하나를 확정 |

`XGBoost 또는 LightGBM`, `React 또는 Next.js`는 허용 후보가 확정된 상태이며 최종 선택은 후속 검증 Decision으로 남아 있다. Kafka, OPA, PostgreSQL, pgvector, MinIO, OpenTelemetry, Prometheus, Grafana와 Docker Compose는 Phase 0 아키텍처 기준이다.
