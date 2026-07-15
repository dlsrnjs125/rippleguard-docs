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
| Shadow Mode | 운영 결정에는 영향을 주지 않고 결과와 차이만 기록하는 Agent 실행 모드 |
| Manual Safe Mode | 자동화 범위를 중단하거나 축소하고 사람의 확인을 필수로 하는 안전 경로 |
| Golden Case | 예상 입력·출력·근거가 검증되어 회귀 및 Replay 비교 기준으로 사용하는 사례 |
| Replay | 저장된 입력과 버전을 이용해 실행을 재현하고 결과 차이를 비교하는 과정 |
