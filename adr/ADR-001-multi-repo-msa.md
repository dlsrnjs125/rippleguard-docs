# ADR-001: Multi-Repo MSA

- Status: Accepted
- Date: 2026-07-14

## Context

RippleGuard는 계약, 인프라, 업무 서비스, Agent, 감사, Web의 배포와 책임 경계를 명확히 하면서 개인 계정에서 관리할 필요가 있다.

## Decision

독립 Repository별로 빌드·테스트·배포하고 Docker Compose로 통합한다. 전체 기준은 `rippleguard-docs`, 실행 가능한 통신 계약은 `rippleguard-contracts`에서 중앙 관리한다.

## Alternatives

- Monorepo: 원자적 변경과 탐색은 쉽지만 독립 배포·책임 경계 연습과 형상 조합 검증이 약해진다.
- 단일 Spring Backend와 Python Runtime: 초기 구성이 단순하지만 업무, Governance, 감사의 데이터·권한 경계가 결합된다.

## Consequences

서비스 독립성과 경계가 명확해지는 대신 Cross-Repo 계약 호환성, Commit Baseline, 통합 자동화 관리 비용이 생긴다.

## Validation

Phase별 Baseline에서 독립 빌드와 호환 Commit 조합을 검증한다.

## Related Documents

- [Repository Strategy](../foundation/repository-strategy.md)
- [Cross-Repo Versioning](../governance/cross-repo-versioning.md)
