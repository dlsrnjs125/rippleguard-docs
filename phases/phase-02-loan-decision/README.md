# Phase 2 — Loan Decision Agent

- Phase: 2
- 상태: `READY`
- 선행 Phase: Phase 1
- 후속 Phase: Phase 3, Phase 4

## 목표와 필요성

Mock 평가를 정형 금융 Snapshot 기반의 재현 가능한 Loan Proposal로 교체한다. 후속 Agent가 검증할 안정적인 Decision Envelope와 Model provenance를 제공한다.

2026-07-20 기준 Phase 1 Core MSA Foundation이 `VERIFIED`이며 Versioned Snapshot, 평가 요청·결과 경계, Kafka·PostgreSQL Infra, commit-tagged OCI Image Baseline과 최소 Case Timeline이 확인되어 Phase 2는 `READY`다.

## 범위와 경계

- 포함: 합성 대출 데이터, Feature Schema, XGBoost·LightGBM 비교, Loan Model 선택, SHAP, Loan Proposal, Model Artifact, `TabularModelPort`, Agent Run 공통 Metadata, Timeout·Retry
- 제외: 실제 Ollama 추론, Local LLM Model 선택, RippleGuard Prompt, Evidence Prompt, 외부 LLM API, Consequence·Evidence 판단, 최종 대출 상태 변경, 금리 최적화
- 변경 금지: Agent는 제안만 생성하며 Governance·Loan 상태와 정책 결과를 직접 변경하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | Feature·Decision Envelope와 Agent Event |
| `rippleguard-agent-runtime` | 모델·SHAP·Loan Decision Agent 실행 |
| `rippleguard-governance-service` | 요청 계획, Output 검증과 Case 연결 |
| `rippleguard-audit-replay-service` | Agent Run·Version Metadata와 최소 Timeline 확장 |
| `rippleguard-infra` | Agent Image와 통합 환경 |

## 선행 조건과 산출물

Phase 1 평가 요청·결과 경계와 Versioned Snapshot이 필요하다. 산출물은 Feature Schema, Model Artifact·Version, Decision Envelope, Agent Docker Image, 재현성 결과와 `agentRunId`·Model·Prompt·Policy Version Trace다.

## 통합 지점과 실패 경로

Governance가 고정 Snapshot·Model Version으로 평가를 요청하고 Agent Runtime이 Decision Envelope를 반환한다. Timeout, Schema 불일치, Model 누락 시 Mock 또는 임의 결과로 대체하지 않고 검증 필요 상태로 축소한다.

## 완료 기준

- Loan Decision Agent는 LLM 없이 동작한다.
- Local LLM Runtime이 없어도 Phase 2 E2E가 성공한다.
- Phase 3 확장 지점을 Architecture Note로만 기록하며 `StructuredLlmPort`의 executable interface는 Phase 3에서 확정한다.
- 동일 Snapshot·Model Version에서 허용 오차 내 Loan Proposal 재현
- SHAP·Evidence Reference와 실행 Version Trace 연결
- 최소 Timeline에서 Evaluation Run과 Agent Run 조회 가능
- Timeout·Retry·Schema 실패 검증
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

RippleGuard와 Evidence Agent가 독립 소비할 검증된 Decision Envelope와 목적 제한 Snapshot이 준비되어야 한다.
