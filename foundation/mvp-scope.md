# MVP Scope

## 대상

| 항목 | 기준 |
| --- | --- |
| 상품 | 프리랜서·플랫폼 노동자 대상 개인신용대출 1종 |
| 신청자 유형 | 프리랜서·플랫폼 노동자 |
| 데이터 | 실제 고객정보가 아닌 합성 금융·문서·FDS 데이터 |

## 대출 결과

- `APPROVE`: 승인 조건과 Assurance가 모두 충족됨
- `CONDITIONAL_APPROVE`: 명시된 조건 충족을 전제로 승인 가능
- `REQUEST_MORE_EVIDENCE`: 확정되지 않은 사실 또는 증빙 보완 필요
- `REJECT`: 대출 정책 결과상 거절; Governance Hard Block과 동일한 의미가 아님

Agent가 결과를 직접 확정하지 않는다. Governance Service가 실행 경로를 검증하고 Loan Service만 최종 대출 결과를 반영한다.

## 핵심 데이터와 문서

| 구분 | 포함 항목 |
| --- | --- |
| 금융·업무 데이터 | 최근 12개월 소득, 소득 변동성, 부채, 연체, 플랫폼 정산, 목적·범위 메타데이터가 있는 FDS 경보 |
| 제출 문서 | 플랫폼 정산명세서, 프리랜서 계약서, 소득 신고자료, 계좌 입금내역 |

원문 전체를 Agent 기본 Context로 전달하지 않으며 [Security and Privacy](../architecture/security-and-privacy.md)의 마스킹·최소 Context 원칙을 적용한다.

## 포함 기능

- Loan Decision Agent, RippleGuard Agent, Evidence & Control Agent
- 결정론적 Governance Service와 OPA 정책 평가
- 관리자 Agent Registry와 `ACTIVE` / `SHADOW_ONLY` / `DISABLED` 통제
- Manual Safe Mode와 Segment Isolation
- Run Trace, Golden Case Replay, Version Diff
- Phase 6 Static Architecture Graph와 Phase 7 EvaluationRun Execution Graph
- FDS Simulator / External Risk Signal Source
- 실행 Trace와 Tamper-Evident Audit Log

## 테스트 범위

- 대표 Demo Case: 6~8건
- Golden Case: 20~30건
- 세 핵심 위험의 Positive·Negative Case와 문서 Prompt Injection Case 포함

대표 Demo의 공통 이름과 기대 경로는 [Canonical Scenarios](canonical-scenarios.md)를 따른다.

## 평가 Dataset 분리

- **Development Case**: Agent·Policy 개발, 오류 분석과 Prompt 수정에 사용
- **Regression Case**: 일반 회귀 검증에 사용하며 변경마다 실행 가능
- **Holdout Evaluation Case**: Phase 9 전까지 결과를 기준으로 Prompt·Policy·Trigger를 튜닝하지 않고 최종 비교 평가에만 사용

Case ID, Partition, Label Version과 접근 이력을 고정한다. Holdout 실패를 보고 난 뒤 수정한 결과는 원래 Holdout 성능과 구분해 보고한다.

정확한 Case 구성, 라벨과 목표 임계치는 평가 Phase에서 고정하되 위 수량 범위와 위험 유형은 MVP 개발 기준으로 사용한다.

## 제외 범위

- 실제 대출금 지급과 실제 금융기관 연동
- 복수 금융기관 또는 복수 대출상품
- 금리 최적화와 복잡한 법률 자문·소비자 권리구제 문서 생성
- Agent Builder와 실시간 Prompt 편집
- Training Data·전체 Data Provenance Graph
- 원문 Prompt·문서 원문·전체 금융 Snapshot을 펼치는 Graph View
- 3D·VR·AR Graph
- Kubernetes 배포

제외 항목을 후속 Phase에 편입하려면 별도 범위 결정과 ADR이 필요하다.
