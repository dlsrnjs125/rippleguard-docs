# Phase 3 — RippleGuard Consequence Agent

- Phase: 3
- 상태: `PLANNED`
- 선행 Phase: Phase 2
- 후속 Phase: Phase 4, Phase 5

## 목표와 필요성

Loan Decision이 데이터 목적·의미·판단 범위를 바꾸며 만드는 2차 위험을 탐지해 Consequence Envelope로 반환한다. Phase 3은 Local LLM이 처음 실제 통합되는 Phase다.

## 범위와 경계

- 포함: Cross-Purpose Data Reuse, Scope Escalation, Semantic Transformation, 만료·미확정 추론 사용, FDS 대표 Case, Ollama Runtime, Local Instruct Model 후보 비교, Quantized Model, `StructuredLlmPort` Ollama Adapter, Prompt Version, Model Manifest, Context Budget, Structured Output, Timeout·Retry
- 제외: Evidence 문서 검증, OPA 최종 실행 경로, 법률 자문, 외부 유료 API 필수 경로
- 변경 금지: RippleGuard Agent는 Loan 결과·Assurance를 확정하거나 Evidence Agent 결론을 입력으로 사용하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | Consequence Envelope와 Event Schema |
| `rippleguard-agent-runtime` | RippleGuard Agent Graph·Tool·검증 |
| `rippleguard-governance-service` | 실행 계획, 결과 Validator와 Case 연결 |
| `rippleguard-infra` | REST FDS Simulator Container와 고정 Test Fixture |

## 선행 조건과 산출물

검증된 Decision Envelope, FDS 목적 Metadata와 Development·Regression 위험 Case가 필요하다. Phase 3은 `StructuredLlmPort` executable interface, Agent Failure Event Schema, Failure Code taxonomy, retryable/non-retryable 분류, Governance route mapping과 Audit storage contract를 먼저 확정한 뒤 OllamaAdapter와 failure drill을 구현한다. 산출물은 Consequence Envelope, Agent Output 검증, 위험 유형별 Test와 Image다. Phase 9 Holdout 결과를 Prompt·Trigger·Policy 튜닝에 사용하지 않는다.

## 통합 지점과 실패 경로

Governance가 Decision Envelope와 허용 Snapshot으로 실행을 지시하고 Agent Runtime이 결과를 반환한다. 목적 Metadata 누락, 만료, Schema 실패 시 안전하다고 추정하지 않고 검증 필요 또는 차단 후보로 기록한다.

## 완료 기준

- 외부 유료 API 없이 Consequence Envelope 생성
- Model Weight가 Git에 포함되지 않음
- Model Name, source revision, artifact digest, provider manifest digest, Modelfile digest, Quantization, Runtime Version 고정
- Model source, source revision, artifact digest target, provider manifest digest, Modelfile digest and license metadata fixed
- Output Schema Validation PASS
- Timeout·Malformed Output·Runtime Down·Digest Mismatch·Context Overflow 안전 실패
- Golden Case와 Negative Case 품질 측정
- FDS 정보 오용 대표 Case를 위험 유형과 Evidence Reference로 탐지
- 허용 사용 Negative Case의 과탐 결과 기록
- Agent Timeout·Schema·Tool 실패 경로 검증
- 독립 빌드·Docker 통합·Baseline 고정

구체적인 Local LLM 모델 이름은 지금 확정하지 않는다. Phase 3 구현 시 후보 2~3개를 한국어 이해, JSON structured output 성공률, Consequence recall, false positive rate, 평균·P95 latency, peak memory, context limit, license, quantization별 품질 저하, cold start와 반복 실행 안정성 기준으로 비교하고 하나를 선택한다.

## 다음 Phase 인계 조건

Evidence Agent와 독립 비교 가능한 Versioned Consequence Envelope와 위험 라벨 Case가 준비되어야 한다.
