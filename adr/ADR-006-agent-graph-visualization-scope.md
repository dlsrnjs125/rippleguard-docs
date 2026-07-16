# ADR-006: Agent Graph Visualization Scope

- Status: Accepted
- Date: 2026-07-16

## Context

RippleGuard는 Multi-Agent MSA 구조, Event 기반 실행 경로, Governance 정책, Audit Trace가 결합된 시스템이다. 텍스트와 표만으로는 서비스·Agent·Tool·Policy 관계와 특정 EvaluationRun의 인과 경로를 빠르게 이해하기 어렵다.

동시에 모든 학습 데이터, 문서, Prompt, 로그를 Graph로 펼치면 노드 수가 폭증하고 개인정보·금융정보 노출 위험이 커진다. Graph는 설명과 추적을 위한 도구이지 새로운 Source of Truth가 아니다.

## Decision

- `react-force-graph-2d`를 사용한다.
- Phase 6에서 Static Architecture Graph를 구현한다.
- Phase 7에서 EvaluationRun Execution Graph를 구현한다.
- Data Provenance·Training Data Graph는 MVP에서 제외한다.
- Graph Data는 Versioned Manifest 또는 Audit Read Model에서 공급한다.
- Web은 운영 DB와 Kafka를 직접 조회하지 않는다.
- Timeline/List 대체 화면을 유지한다.

## Alternatives

### 3D Graph

시각적 효과는 크지만 정보 탐색, 접근성, 구현 안정성 측면에서 MVP 우선순위가 낮다. `react-force-graph-3d`, VR, AR, Three.js 기반 3D Graph는 MVP에서 제외한다.

### Cytoscape 또는 직접 SVG 구현

정교한 Graph 분석 기능이나 완전한 DOM 제어에는 장점이 있지만, RippleGuard MVP의 요구는 2D Canvas 기반 Zoom·Pan·Node Drag·Hover·Click·Custom Drawing·Directional Animation에 가깝다. 구현 Phase에서 문제가 발견되면 후보를 재검토할 수 있다.

### 모든 로그와 Data Provenance를 하나의 Graph로 통합

단일 화면에서 많은 정보를 볼 수 있지만 실행 통제 경로가 흐려지고 보안 위험이 커진다. 원문 Prompt, 문서 원문, 전체 Snapshot과 전체 로그 Event는 Graph Payload에 포함하지 않는다.

### Graph 화면 없이 Timeline만 사용

접근성과 정확한 순서 표현에는 충분하지만 Multi-Agent 관계와 인과 경로를 직관적으로 설명하기 어렵다. Timeline은 대체 화면으로 유지하고 Graph는 보조 탐색 View로 추가한다.

## Consequences

### 장점

- 전체 구조와 실행 인과관계를 직관적으로 설명할 수 있다.
- 포트폴리오 데모 가치가 높다.
- Agent, Policy, Event 경계를 시각적으로 드러낼 수 있다.

### 비용

- Graph Read Model이 필요하다.
- Node 수와 렌더링 성능을 통제해야 한다.
- Graph가 실제 관계를 왜곡하지 않도록 Manifest, Contracts, Audit Trace와의 검증이 필요하다.
- 접근성을 위해 Timeline/List 대체 화면을 계속 유지해야 한다.
