# Agent Graph Visualization

Agent Graph Visualization은 RippleGuard의 구조와 특정 Evaluation Run의 실행 경로를 설명·추적하기 위한 화면이다. Graph는 새로운 업무 Source of Truth가 아니며, Versioned Manifest 또는 Audit Read Model에서 파생된 View다.

## 목적

- 전체 시스템, Agent, Tool, Policy, Event 관계를 한눈에 설명한다.
- 특정 Case·EvaluationRun의 입력, Agent 실행, 정책 판단, 최종 Routing 인과관계를 추적한다.
- 포트폴리오 방문자에게 Multi-Agent MSA 구조를 시각적으로 전달한다.
- Operator와 Auditor가 Trace와 Governance 판단 경로를 이해하도록 보조한다.

## View 구분

| 구분 | Architecture Graph | Execution Graph |
| --- | --- | --- |
| 기준 | Versioned 정적 관계 | 특정 Case·EvaluationRun |
| 변경 주기 | 배포·계약 변경 시 | Event 발생 시 |
| 주 사용자 | 방문자, Operator, Auditor | Reviewer, Operator, Auditor |
| 데이터 Source | `rippleguard-infra`의 Versioned Topology Manifest | Audit Read Model |
| 주요 목적 | 시스템 이해 | 판단 경로 추적 |

## View A — Static Architecture Graph

Static Architecture Graph는 실시간 서비스 디스커버리가 아니라 Versioned Architecture Topology Manifest를 렌더링하는 설명용 화면이다.

### 표시 대상

- Service
- Agent
- Tool
- OPA Policy
- Kafka Event Topic 또는 Event Type
- 외부 시스템

### 대표 노드

- Loan Service
- Governance Service
- Agent Runtime
- Audit & Replay Service
- Web
- FDS Simulator
- Loan Decision Agent
- RippleGuard Agent
- Evidence & Control Agent
- OPA
- MinIO
- Kafka

### 대표 관계

- `PUBLISHES`
- `SUBSCRIBES`
- `REQUESTS`
- `USES_TOOL`
- `EVALUATED_BY`
- `GOVERNED_BY`
- `STORES_IN`
- `PROVIDES_INPUT_TO`

## View B — EvaluationRun Execution Graph

Execution Graph는 특정 대출 Case 또는 EvaluationRun이 어떤 입력, Agent, Event, 정책을 거쳐 최종 경로에 도달했는지 인과관계로 추적한다.

### 표시 대상

- Case 시작
- Evaluation Run
- Agent Run
- Decision Envelope
- Consequence Envelope
- Evidence Finding
- Policy Decision
- Human Verification Task
- 최종 Loan Decision
- Event

### 대표 관계

- `CAUSED`
- `TRIGGERED`
- `PRODUCED`
- `CONSUMED`
- `VALIDATED`
- `SUPERSEDES`
- `REQUESTED_EVIDENCE`
- `ROUTED_TO`
- `FINALIZED_AS`

Execution Graph 링크는 `correlationId`, `causationId`, `evaluationRunId`, `agentRunId`를 중심으로 구성한다. 실제 Event causation과 화면 탐색용 파생 관계는 `linkOrigin`, `causalityStatus`, `evidenceRefs`로 구분한다. 실제 Event 이동은 선택된 링크 또는 재생 중인 링크에만 방향성 Particle Animation을 적용한다. 모든 링크에 계속 Particle을 표시하지 않는다.

## Repository 책임

| Repository | 책임 |
| --- | --- |
| `rippleguard-docs` | 개념, 범위, Roadmap, 완료 기준 |
| `rippleguard-contracts` | Topology Manifest Schema, 공통 Graph Node·Link Core DTO, Architecture·Execution Graph 확장 DTO와 조회 API 계약 |
| `rippleguard-infra` | Versioned Topology Manifest 원본 소유, 실제 Compose·Kafka Topic·OPA 배치와 정적 검증, Manifest 배포 |
| `rippleguard-web` | `react-force-graph-2d` 렌더링, 필터, 검색, 상세 패널, 접근성 |
| `rippleguard-audit-replay-service` | EvaluationRun Execution Graph Read Model, `correlationId`·`causationId` 기반 Graph 조회 |
| `rippleguard-governance-service` | Agent Registry Snapshot과 EvaluationRun Reference 제공; Topology Manifest 원본은 소유하지 않음 |

## 데이터 Source 원칙

### Architecture Graph

Phase 6에서는 `rippleguard-web`이 `rippleguard-infra`가 소유·배포하는 Versioned Topology Manifest를 이용해 렌더링한다. 이 Manifest는 발표용 임의 데이터가 아니라 다음 기준과 대조할 수 있어야 한다.

- `rippleguard-infra` 서비스 구성
- `rippleguard-contracts` Event 목록
- Governance Agent Registry
- Docs Integration Matrix

#### Manifest 소유권과 버전 규칙

| 항목 | 기준 |
| --- | --- |
| Manifest Owner Repository | `rippleguard-infra` |
| 계약 Owner Repository | `rippleguard-contracts` |
| Consumer | `rippleguard-web` |
| Agent Registry Source | `rippleguard-governance-service`의 Agent Registry Snapshot |
| Manifest 원본 위치 | `rippleguard-infra`의 Phase 6 구현에서 확정하되, Web 내부 하드코딩은 금지 |
| 필수 Metadata | `manifestVersion`, `generatedAt`, `effectiveFrom`, `contractsCommit`, `infraCommit`, `agentRegistryVersion` |
| 검증 명령 | `rippleguard-infra`에서 Compose/Kafka Topic/OPA 배치와 Contracts Schema를 대조하는 정적 검증 명령 |
| 배포 방식 | Infra가 빌드·배포 Artifact 또는 정적 API로 제공하고 Web은 Consumer로만 사용 |

Manifest는 여러 Source에서 생성·검증된 Published Topology Artifact다. `rippleguard-infra`는 Artifact Owner이지만 모든 관계의 업무 원본은 아니다.

Field-level Source는 다음과 같이 구분한다.

- Event and DTO: `rippleguard-contracts`
- Service and Topic deployment: `rippleguard-infra`
- Agent Registry: `rippleguard-governance-service`
- Architecture intent: `rippleguard-docs`

`generatedAt`은 Artifact 생성 시각이고 `effectiveFrom`은 해당 Topology 기준이 유효해지는 시각이다. 과거 Manifest Replay와 Version Diff에서는 두 값을 구분한다.

Manifest는 실제 Compose 서비스, Kafka Topic, OPA 배치, Contracts Event 목록, Agent Registry Snapshot과 대조 가능해야 한다. Governance Service는 Agent Registry Snapshot을 제공하지만 Manifest 파일의 원본 Repository가 아니다.

### Execution Graph

Phase 7에서는 Audit & Replay Service가 Case·EvaluationRun 단위 Graph Read Model을 제공한다. Web이 Kafka나 각 서비스 DB에 직접 접근해서 그래프를 만들지 않는다.

## UI·UX 기준

### 공통

- 기본은 2D Graph다.
- Zoom, Pan, Node Drag, Fit to View, Reset Layout을 제공한다.
- Search, Node Type Filter, Link Type Filter, 상태별 Filter를 제공한다.
- Legend를 제공한다.
- 선택 Node 중심 Highlight를 제공하고 선택되지 않은 연결은 흐리게 표시한다.
- 상세 정보는 우측 Side Panel에 표시한다.
- 그래프 위에 긴 본문을 직접 표시하지 않는다.

### Architecture Graph

- 기본 화면에는 핵심 서비스만 표시한다.
- Agent·Tool·Policy는 선택 시 확장한다.
- Event Topic은 기본 축약 가능하다.
- 물리 서비스와 논리 Agent를 시각적으로 구분한다.
- Pub/Sub 방향을 Arrow로 표시한다.
- 모든 Event에 Particle Animation을 사용하지 않는다.

### Execution Graph

초기 진입은 다음 순서를 따른다.

```text
Case ID 또는 EvaluationRun ID 검색
→ 전체 Run 목록
→ 하나의 EvaluationRun 선택
→ 해당 Run Graph 표시
```

Execution Graph는 다음 기능을 제공한다.

- 인과 순서 기준 Highlight
- 시간순 재생
- 실행 성공·실패·생략 상태 구분
- 실제 `causationId` Edge, Reference 기반 파생 Edge, Static Topology Edge를 시각적으로 구분
- Trigger로 실행되지 않은 Agent의 생략 이유 표시
- 이전 Run과 `supersedesRunId` 연결
- 특정 Node 클릭 시 Version·Reason·Reference 표시
- 필요 시 Timeline List와 Graph를 함께 제공

Graph만으로 모든 정보를 전달하지 않는다. 접근성과 정확한 시간 순서를 위해 Timeline 또는 List 대체 화면을 유지한다.

## 복잡도와 성능 Budget

초기 Budget은 개발 Phase에서 측정 후 조정할 수 있으며, 변경 시 Roadmap Change Log 또는 구현 문서에 기록한다.

| View | Budget |
| --- | --- |
| Architecture Graph | 기본 표시 Node 30개 이하 권장 |
| Architecture Graph | 확장 후 Node 60개 이하 권장 |
| Execution Graph | 최초 표시 Node 100개 이하 |
| Execution Graph | 최초 표시 Link 150개 이하 |

Execution Graph가 Budget을 초과하면 Event Type, 시간 범위, Agent Run Filter를 먼저 요구한다. 대량 Trace를 한 번에 전부 표시하지 않는다.

Progressive Disclosure 순서는 다음과 같다.

```text
Case
→ EvaluationRun
→ Agent Run
→ Envelope·Finding
→ 선택된 Event 상세
```

추가 기준은 다음과 같다.

- 전체 Graph Dataset을 매 Render마다 재생성하지 않는다.
- Node·Link ID 안정성을 유지한다.
- Graph Response Envelope의 `isTruncated`, `traceCompleteness`, `appliedFilters`, `omittedNodeCount`, `omittedLinkCount`, `nextCursor`, `warnings`를 통해 부분 Graph와 완전한 Trace를 구분한다.
- Particle은 선택·재생 구간에만 사용한다.
- 서버 DTO는 `animationEligible`만 제공할 수 있으며, 실제 `animated` 상태는 Web이 선택 Link, Playback Cursor, Reduced Motion 설정에 따라 계산한다.
- 정적 Graph는 초기 Layout 안정화 후 불필요한 Simulation을 멈출 수 있다.
- 상세 원문은 Graph Payload에 포함하지 않는다.

## MVP 제외 범위

Data Provenance / Training Data Graph는 MVP에서 제외한다.

제외 대상은 다음과 같다.

- 전체 학습 Dataset
- 모든 RAG Chunk
- Embedding 간 관계
- 학습 샘플 단위
- Prompt 전체 본문
- 고객 문서 원문
- 전체 금융 Snapshot 필드
- 모든 로그 Event

제외 이유는 다음과 같다.

- 노드가 과도하게 증가한다.
- 주요 의사결정 경로의 가독성이 저하된다.
- 개인정보와 금융정보 노출 위험이 커진다.
- 학습 데이터 계보는 현재 MVP의 핵심 문제인 실행 통제와 직접 관련성이 낮다.
- Agent 학습·모델 계보는 별도 MLOps 범위를 요구한다.

다만 실행 그래프의 상세 패널에는 다음 Reference만 제한적으로 표시할 수 있다.

- `snapshotVersion`
- `documentId`
- `evidenceRef`
- `modelVersion`
- `promptVersion`
- `policyVersion`
- `toolVersion`

Reference는 원문이 아니며 클릭 시에도 권한 검증을 거쳐야 한다.
