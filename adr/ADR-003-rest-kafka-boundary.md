# ADR-003: REST와 Kafka 경계

- Status: Accepted
- Date: 2026-07-14

## Context

사용자 상호작용과 즉시 조회에는 즉각적인 응답이 필요하지만 Agent 평가와 서비스 간 상태 변경은 지연·재시도·감사 추적이 필요하다.

## Decision

사용자 요청과 즉시 조회는 REST를 사용한다. 서비스 간 상태 변경과 비동기 업무는 Kafka를 사용한다. 모든 통신을 Kafka로 통일하지 않으며 OPA 정책 평가는 동기 호출로 처리한다.

## Alternatives

- 전부 REST: 단순하지만 장시간 처리와 재시도에서 시간 결합이 커진다.
- 전부 Kafka: 상태 변경에는 적합하지만 즉시 조회와 사용자 오류 응답이 복잡해진다.

## Consequences

용도에 맞는 통신을 선택할 수 있지만 두 방식의 계약, 관측, 실패 처리 기준을 함께 운영해야 한다.

## Validation

Integration Matrix와 E2E 검증에서 각 경계의 시간 특성과 장애 경로를 확인한다.

## Related Documents

- [Data Flow](../architecture/data-flow.md)
- [Integration Matrix](../tracking/integration-matrix.md)
