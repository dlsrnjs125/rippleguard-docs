# Phase 4 — Evidence & Legacy Control

- Phase: 4
- 상태: `PLANNED`
- 선행 Phase: Phase 2, Phase 3
- 후속 Phase: Phase 5

## 목표와 필요성

정형 데이터만으로 확인할 수 없는 증빙 불일치와 기존 필수 통제 누락을 Decision과 독립적으로 검증한다.

## 범위와 경계

- 포함: MinIO, 안전한 문서 파싱, pgvector, 정산서·계약서·입금내역, Legacy Control Registry, 정형·비정형 비교
- 제외: 법률 자문, 이의신청서 생성, Consequence Agent 결론 소비
- 변경 금지: 업로드 문서는 명령이 아니며 Agent Context·Tool 권한을 바꿀 수 없다. Agent Runtime은 업무 DB에 직접 접근하지 않는다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-contracts` | Evidence Finding·Control Coverage 계약 |
| `rippleguard-agent-runtime` | Evidence & Control Agent와 문서 검증 |
| `rippleguard-infra` | MinIO·pgvector·문서 처리 환경 |
| `rippleguard-governance-service` | Schema 검증, 채택된 Finding Snapshot의 Versioned 보존과 Evaluation Run 연결 |

## 선행 조건과 산출물

Decision Envelope, 목적 제한 Object Reference, 문서 보안 원칙이 필요하다. 산출물은 Findings Schema, Control Registry, 문서 Test Fixture, Agent Image와 검증 결과다.

Evidence & Control Agent는 초기에는 Phase 3에서 검증한 Local LLM Runtime과 Model을 공유한다. 별도 모델 도입은 문서 분석 정확도, Latency, Memory와 Common-Mode Failure를 비교한 뒤 결정한다.

## 통합 지점과 실패 경로

Governance가 필요한 문서 Chunk와 Control 목록으로 독립 실행을 요청한다. 파싱 실패, Prompt Injection 의심, Evidence 누락 시 원문을 Trace에 복사하지 않고 검증 필요 상태와 Reference를 반환한다.

## 완료 기준

- 동일 Model 공유 상태에서도 Prompt·Input·Tool·Output이 독립적임
- Prompt Injection Test PASS
- Context Overflow Test PASS
- Parser 오류와 LLM 판단 오류를 분리 측정
- 문서 원문과 Chain-of-Thought 비저장
- 누락 소득 증빙과 필수 통제 누락을 구조화된 Finding으로 생성
- Prompt Injection·민감정보 Trace 비노출 테스트 통과
- Parser Ground Truth 정확도와 Evidence Agent 판단 정확도를 분리 측정
- Consequence 결과와 독립 실행 확인
- Agent 재파싱·Version 변경 후에도 과거 Evaluation Run의 채택 Finding Snapshot 재현
- Docker 통합과 Baseline 고정

## 다음 Phase 인계 조건

OPA가 사용할 Versioned Evidence Findings와 Legacy Control Coverage가 준비되어야 한다.
