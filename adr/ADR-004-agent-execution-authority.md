# ADR-004: Agent 실행 권한

- Status: Accepted
- Date: 2026-07-14

## Context

확률적 Agent가 최종 금융 상태를 직접 변경하면 설명되지 않은 판단, 증거 부족, 정책 우회가 즉시 고객 영향으로 이어질 수 있다.

## Decision

Agent는 제안과 분석만 수행한다. Governance Service가 Schema, Evidence, Assurance와 OPA 결과를 검증해 실행 경로를 결정하고, Loan Service만 최종 대출 상태를 변경한다. 정책 엔진을 우회하는 경로는 허용하지 않는다.

## Alternatives

- Agent의 직접 상태 변경: 자동화는 단순해지지만 결정론적 통제와 권한 분리가 사라진다.
- 사람의 전건 승인: 안전하지만 MVP가 검증하려는 조건부 자동화와 확장성을 평가하기 어렵다.

## Consequences

안전 경계와 감사 가능성이 높아지는 대신 Envelope 검증, 정책 운영, 상태 조정 로직이 추가된다.

## Validation

권한 테스트에서 Agent Tool이 업무 DB와 최종 상태 API에 접근할 수 없고 장애 시 자동화가 축소되는지 확인한다.

## Related Documents

- [Agent Governance](../architecture/agent-governance.md)
- [Loan Case Lifecycle](../domain/loan-case-lifecycle.md)
