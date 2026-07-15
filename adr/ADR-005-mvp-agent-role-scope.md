# ADR-005: MVP Agent 역할 범위

- Status: Accepted
- Date: 2026-07-15

## Context

초기 구상에는 소비자 권리구제와 법률 자문을 담당하는 Rights Advisor Agent가 포함됐다. 그러나 이 역할은 결정 이후의 이의신청·구제 문서 생성까지 범위를 넓혀, MVP가 검증할 결정 이전의 2차 위험과 Legacy Control 통제를 흐릴 수 있다.

## Decision

Rights Advisor의 소비자 권리구제 기능은 후속 범위로 미룬다. MVP에서는 Evidence & Control Agent가 결정 이전의 증빙 완전성, 출처, Legacy Control Coverage를 독립 검증한다. 복잡한 법률 자문과 이의신청서 작성은 수행하지 않는다.

## Alternatives

- Rights Advisor를 MVP에 포함: 사용자 권리 관점을 넓히지만 법률 정확성, 관할, 승인 책임과 데이터 범위가 크게 증가한다.
- Evidence 검증도 Governance Service만 수행: 결정론적 통제는 단순하지만 문서·증빙 분석을 독립 전문 Agent로 검증하기 어렵다.

## Consequences

MVP Agent 책임과 평가 범위가 선명해진다. 반면 결정 이후의 설명·이의제기·구제 흐름은 제공하지 않으며 후속 범위 결정이 필요하다. 외부 기획 문서의 Agent 명칭과 설명도 Evidence & Control Agent로 맞춰야 한다.

## Validation

Agent Tool과 Output 계약에 법률 자문·권리구제 문서 생성 기능이 없고, Evidence·Legacy Control 검증만 포함되는지 확인한다.

## Related Documents

- [MVP Scope](../foundation/mvp-scope.md)
- [Agent Governance](../architecture/agent-governance.md)
- [Human Verification](../governance/human-verification.md)
