# Terminology

| 용어 | 정의 |
| --- | --- |
| Decision Case | 한 대출 신청의 제안, Assurance, 정책 판단, 최종 결과를 연결해 추적하는 Governance 단위 |
| Decision Envelope | Loan Decision Agent의 판단 제안과 사용 데이터·근거·버전을 담는 전달 객체 |
| Consequence Envelope | 제안이 데이터 목적과 판단 범위를 어떻게 바꿀 수 있는지 분석한 결과 |
| Assurance Profile | Evidence, 통제 이행, 정책 검증 결과를 종합한 Governance 평가 입력 |
| Cross-Purpose Data Reuse | 한 목적으로 수집한 데이터를 정당화되지 않은 다른 판단 목적에 사용하는 위험 |
| Scope Escalation | 제한된 신호나 사건을 신청자 전체 또는 더 넓은 판단 범위로 확대하는 위험 |
| Semantic Transformation | 원 데이터의 의미가 추론·요약 과정에서 다른 위험 또는 속성으로 변환되는 현상 |
| Legacy Control Loss | 기존 심사에서 필수였던 확인·승인·증빙 절차가 자동화 과정에서 누락되는 위험 |
| Assurance Complete | 필요한 증거와 통제가 충족되어 정책 평가를 계속할 수 있는 상태 |
| Assurance Incomplete | 필요한 증거 또는 통제 확인이 부족해 자동 처리를 계속할 수 없는 상태 |
| Assurance Violated | 명시된 정책이나 통제 조건을 위반한 상태 |
| Agent Run | 특정 Agent·모델·Prompt·Tool·입력 버전으로 수행된 한 번의 실행 |
| Evaluation Run | 한 Decision Case 안에서 고정 Snapshot·실행 계획·Policy Version으로 수행되는 전체 평가 단위; 하나 이상의 Agent Run을 포함 |
| Hybrid Multi-Agent | 정형 ML Loan Decision Agent와 Local LLM 기반 전문 Agent를 Governance 경계로 결합한 구조 |
| Foundation Model | Agent가 공유하거나 개별 사용할 수 있는 기본 추론 Weight와 Runtime 조합 |
| Model | 실제 추론 Weight, Runtime, Quantization과 Version Metadata를 가진 실행 Artifact |
| Agent | 목적, 입력 계약, Prompt, Tool Allowlist, Output Contract와 Run ID를 가진 실행 단위 |
| Agent Runtime | Agent Graph, Tool 호출, 실행 상태와 Agent Run을 관리하는 서비스 |
| Local LLM Runtime | 모델을 메모리에 로드하고 로컬 추론 API를 제공하는 실행 환경 |
| Model Provider | Local 또는 외부 모델 추론 Runtime을 제공하는 구현체 |
| StructuredLlmPort | Agent Runtime이 Ollama, llama.cpp, 외부 API를 교체 가능하게 호출하는 구조화 LLM 경계 |
| Ollama Adapter | StructuredLlmPort의 MVP 기본 Local LLM Provider Adapter |
| llama.cpp Adapter | 후속 재현성 강화 또는 GGUF 기반 실행 후보 Adapter |
| External API Adapter | MVP 필수 경로가 아닌 외부 LLM API 비교·Benchmark 후보 Adapter |
| Model Manifest | 모델 이름, source revision, artifact digest, provider manifest digest, Modelfile digest, Quantization, Context, Runtime Version과 license를 기록하는 Versioned JSON 또는 YAML 기준 |
| Model Binary Artifact | Git에 저장하지 않는 실제 추론 파일. 예: GGUF, weights, adapter files |
| Model Governance Artifact | Git에서 추적하는 모델 운영 기준. 예: Manifest, digest semantics, prompt version, evaluation result, license metadata |
| Model Digest | 대상과 알고리즘이 명시된 digest. `artifactDigest`, `providerManifestDigest`, `modelfileDigest` 또는 규칙이 정의된 composite digest를 구분해 기록한다 |
| Quantization | 로컬 실행을 위해 모델 Weight 정밀도를 줄여 메모리와 Latency를 낮추는 방식 |
| Context Budget | Agent 목적에 맞게 전달 가능한 입력 Token 또는 Context 크기 제한 |
| Structured Output | JSON Schema와 Deterministic Validator가 검증할 수 있는 구조화된 모델 출력 |
| Common-Mode Failure | 독립 Agent가 같은 모델, 데이터 또는 Prompt 약점 때문에 같은 오류를 내는 위험 |
| Prompt Version | Prompt 변경을 Replay와 평가에서 구분하기 위한 Version |
| Tool Version | Agent Tool Allowlist와 Tool 구현 변경을 추적하는 Version |
| Shadow Mode | 운영 결정에는 영향을 주지 않고 결과와 차이만 기록하는 Agent 실행 모드 |
| Manual Safe Mode | 자동화 범위를 중단하거나 축소하고 사람의 확인을 필수로 하는 안전 경로 |
| Golden Case | 예상 입력·출력·근거가 검증되어 회귀 및 Replay 비교 기준으로 사용하는 사례 |
| Replay | 저장된 입력과 버전을 이용해 실행을 재현하고 결과 차이를 비교하는 과정 |
