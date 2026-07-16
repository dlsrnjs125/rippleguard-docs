# RippleGuard Docs

RippleGuard는 대출심사 AI의 판단이 목적과 범위를 벗어나거나 기존 내부통제를 잃는 2차 위험을 탐지하고 차단하는 Multi-Agent Loan Decision Governance Platform이다.

이 저장소는 전체 시스템 관점의 기획, 아키텍처, ADR, Phase 진행 상황과 멀티레포 통합 기준을 관리한다. 서비스 구현과 실행 가능한 계약은 각 전용 저장소에서 관리한다.

## 바로가기

- [프로젝트 개요](foundation/project-overview.md)
- [MVP 범위](foundation/mvp-scope.md)
- [Canonical Scenarios](foundation/canonical-scenarios.md)
- [시스템 컨텍스트](architecture/system-context.md)
- [서비스 경계](architecture/service-boundaries.md)
- [기술 스택](architecture/technology-stack.md)
- [Agent Graph Visualization](architecture/graph-visualization.md)
- [Consequence Risk Model](domain/consequence-risk-model.md)
- [Evaluation Run](domain/evaluation-run.md)
- [Graph View Model](domain/graph-view-model.md)
- [Assurance Routing](governance/assurance-routing.md)
- [Security and Privacy](architecture/security-and-privacy.md)
- [Phase 현황](tracking/phase-status.md)
- [Phase 의존성](tracking/phase-dependency-map.md)
- [프로젝트 위험](tracking/project-risk-register.md)
- [Release Readiness](tracking/release-readiness.md)
- [Phase 0](phases/phase-00-foundation/README.md)
- [ADR 목록](adr/README.md)

현재 **Phase 0 — Foundation & Architecture Baseline**은 `IN_REVIEW`이다. Contracts와 Infra 결과는 검증됐고, 이 Docs finalization PR의 merge commit을 main baseline에 고정하는 Metadata PR 이후 `VERIFIED`로 전환한다. 상세 진척과 완료 검증은 [Phase 0 progress](phases/phase-00-foundation/progress.md)와 [verification](phases/phase-00-foundation/verification.md)에서 추적한다.

Agent Graph Visualization은 Phase 6에서 정적 Architecture Graph, Phase 7에서 EvaluationRun Execution Graph로 계획한다. 실제 React 구현, Graph API 구현과 Training Data·전체 Data Provenance Graph는 이번 Roadmap 문서 범위에 포함하지 않는다.

## Repository 구성

`rippleguard-docs`, `rippleguard-contracts`, `rippleguard-infra`, `rippleguard-loan-service`, `rippleguard-governance-service`, `rippleguard-agent-runtime`, `rippleguard-audit-replay-service`, `rippleguard-web`의 책임과 의존성은 [Repository Map](tracking/repository-map.md)에 정리한다.

## Source of Truth

- 전체 시스템 관점과 Phase 기준: `rippleguard-docs`
- API, Event, Agent Output, Policy Input Schema: `rippleguard-contracts`
- 서비스 구현 상세: 각 서비스 Repository
- 통합 실행과 배포 설정: `rippleguard-infra`

문서를 변경할 때는 영향을 받는 Phase와 Repository를 명시하고, 계약이나 아키텍처 결정 변경은 관련 계약 버전 또는 ADR을 함께 갱신한다. 새 Phase는 [Phase 문서 운영 안내](phases/README.md)와 [Phase Template](templates/phase-readme-template.md)을 따른다.
