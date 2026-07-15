# ADR-002: Database per Service

- Status: Accepted
- Date: 2026-07-14

## Context

대출 상태, Governance 판단, Agent 실행, 감사 기록은 변경 권한과 보존 성격이 다르다. 공유 DB는 이 경계를 우회하게 만든다.

## Decision

서비스별 데이터 저장소를 소유하고 다른 서비스 DB 직접 접근을 금지한다. 데이터는 계약된 API, Event 또는 목적이 제한된 Snapshot으로 전달한다.

## Alternatives

- 공유 DB: 초기 조회와 트랜잭션이 단순하지만 소유권과 독립 변경이 무너진다.
- 공통 Entity/Repository 모듈: 중복은 줄지만 저장 모델과 배포가 결합된다.

## Consequences

독립 변경과 권한 통제가 가능해지는 대신 Eventual Consistency, 중복 데이터, 멱등 처리와 분산 추적이 필요하다.

## Validation

통합 검증에서 교차 DB 접근이 없고 API·Event·Snapshot만 사용하는지 확인한다.

## Related Documents

- [Data Ownership](../architecture/data-ownership.md)
- [Service Boundaries](../architecture/service-boundaries.md)
