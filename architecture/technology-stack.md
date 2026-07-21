# Technology Stack

Phase 0에서 언어와 핵심 프레임워크 경계를 고정한다. 구체적인 런타임·라이브러리 버전과 빌드 플러그인은 각 구현 Repository에서 검증 후 확정한다.

| 영역 | 선택 | 적용 범위 |
| --- | --- | --- |
| Loan, Governance, Audit Backend | Spring Boot | REST, Kafka Consumer/Producer, 서비스별 업무 로직 |
| Agent Runtime | Python, FastAPI, LangGraph | Agent 실행 API와 상태 그래프 |
| Loan Model | XGBoost 또는 LightGBM | 합성 데이터 기반 대출 제안 모델; Golden Case 비교 후 하나를 확정 |
| XAI | SHAP | 모델 입력 기여도 설명; Governance 판단을 대체하지 않음 |
| Local LLM Runtime | Ollama | Phase 3·4 비정형 전문 Agent의 로컬 추론 |
| Local LLM Model | Quantized Instruct Model | RippleGuard·Evidence Agent가 초기 공유 |
| Model Provider Abstraction | StructuredLlmPort | Ollama, llama.cpp, 외부 API 교체 경계 |
| Model Manifest | Versioned JSON or YAML Manifest | 모델 이름, source revision, artifact/provider/Modelfile digest, Quantization, Context, Runtime Metadata, license |
| Alternative Runtime | llama.cpp | 후속 재현성·GGUF 고정 실행 후보 |
| Event | Kafka | 서비스 간 상태 변경과 비동기 업무 |
| Policy | OPA | Versioned Assurance Routing 정책 |
| Database | PostgreSQL | 서비스별 독립 DB |
| RAG·Vector | pgvector | Agent Runtime의 목적 제한 검색 파생 데이터 |
| Document Object | MinIO | 사용자 제출 원본과 선택적 대용량 파싱 원본 Artifact; 업무 상태와 Evidence Finding은 PostgreSQL Versioned Record |
| Observability | OpenTelemetry, Prometheus, Grafana | 분산 Trace, Metric, Dashboard |
| Local Integration | Docker Compose | 모든 Image와 플랫폼 구성 통합 |
| FDS Simulator | 경량 Mock Server 또는 WireMock | 운영형 Demo REST Source; 고정 JSON Fixture는 Test 전용 |
| Web | React 또는 Next.js | 신청·심사·Governance Console; Phase 6 전에 하나를 확정 |
| Graph Visualization | react-force-graph-2d | Phase 6 정적 Architecture Graph와 Phase 7 EvaluationRun Execution Graph |

`XGBoost 또는 LightGBM`, `React 또는 Next.js`는 허용 후보가 확정된 상태이며 최종 선택은 후속 검증 Decision으로 남아 있다. Graph Visualization의 default implementation choice는 `react-force-graph-2d`이며 구체적인 라이브러리 버전은 `rippleguard-web` 구현 Phase에서 검증 후 확정한다. Kafka, OPA, PostgreSQL, pgvector, MinIO, OpenTelemetry, Prometheus, Grafana와 Docker Compose는 Phase 0 아키텍처 기준이다.

Phase 6 spike는 접근성, 100-node 성능, Next.js/React 통합, Reduced Motion 동작을 검증한다. 검증 실패 시 ADR amendment로 대체 기술을 검토한다.

MVP Graph는 2D Canvas 기반 정보 탐색을 우선한다. `react-force-graph-3d`, VR, AR, Three.js 기반 3D Graph는 시각 효과보다 가독성, 접근성, 구현 안정성을 우선하기 위해 MVP에서 제외한다.

Loan Decision Model은 LLM이 아니다. Local LLM은 Loan Score 또는 최종 대출 결정을 계산하지 않는다. MVP의 기본 LLM Provider는 Ollama이며 RippleGuard와 Evidence Agent는 초기에는 동일한 Local Foundation Model을 공유할 수 있다. 외부 API와 llama.cpp는 Adapter를 통해 교체 가능한 후속 후보다. 구체적인 Local LLM 모델 이름과 버전은 이 문서에서 확정하지 않고 Phase 3 구현 검증 결과로 고정한다.
