# Project Overview

## 문제 정의

기존 금융 AI는 예측 정확도와 자동화율을 중심으로 평가되는 경우가 많다. 그러나 개별 판단이 정확하더라도, 원래 다른 목적으로 수집한 데이터가 상환능력 평가에 재사용되거나 한 거래의 이상 신호가 신청자 전체의 신용위험으로 확대되고, 자동화 과정에서 기존 필수 확인 절차가 사라질 수 있다. RippleGuard는 이러한 **AI 판단의 2차 위험**을 통제한다.

## 해결 방식

RippleGuard는 대출 판단을 Decision Case로 추적하고, Loan Decision Agent의 제안에 대해 RippleGuard Agent가 목적·범위의 변질을 분석하며 Evidence & Control Agent가 증거와 기존 통제 이행 여부를 확인한다. 결정론적 Governance Service와 정책 엔진은 결과를 검증해 자동 처리, 추가 검증 또는 차단 경로를 선택한다. 사람은 고위험·불완전 사례를 확인하고 Agent 상태와 정책 활성화를 통제한다.

대출 신청은 데이터 목적, 판단 범위, 설명 가능성, 기존 내부통제가 한 흐름에서 충돌하는 대표적 실증 도메인이므로 MVP 대상으로 선택한다. Multi-Agent는 대출 제안, 2차 위험 검토, 증거·통제 검증을 서로 다른 책임으로 분리해 한 Agent의 결론이 자체 검증으로 끝나지 않게 한다.

정책 엔진은 사전에 승인된 결정 규칙을 일관되게 집행하고, 사람은 정책으로 확정할 수 없는 사례의 검증과 변경 승인에 책임을 진다. Agent는 분석과 제안만 수행하며 최종 금융 행동을 실행하지 않는다.

## MVP 성공 기준

- Cross-Purpose Data Reuse, Scope Escalation, Legacy Control Loss를 재현 가능한 Case로 탐지한다.
- 모든 자동 결정은 검증된 계약, Evidence Reference, 정책 결과와 Trace로 설명할 수 있다.
- 불완전하거나 위반된 Assurance는 추가 검증 또는 차단으로 안전하게 축소된다.
- Replay로 Agent·Prompt·정책 버전 변화에 따른 결과 차이를 확인할 수 있다.
- 서비스 데이터 소유권과 최종 상태 변경 권한이 경계를 넘어가지 않는다.
