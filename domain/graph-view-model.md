# Graph View Model

이 문서는 Agent Graph Visualization을 설명하기 위한 View Model을 정의한다. 실행 가능한 Schema가 아니며, 최종 Graph Node·Link DTO와 조회 API의 Source of Truth는 향후 `rippleguard-contracts`에서 관리한다.

Graph View Model은 화면 렌더링과 문서화 기준을 맞추기 위한 중간 설계다. 서비스 상태 변경, 정책 판단, Event 확정의 Source of Truth가 아니다.

## Graph Node 개념

최소 필드는 다음과 같다.

| 필드 | 의미 |
| --- | --- |
| `id` | Graph 내부에서 안정적인 Node 식별자 |
| `nodeType` | Node 유형 |
| `label` | 화면 표시용 짧은 이름 |
| `group` | 서비스, Agent, 정책, Event 등 시각적 그룹 |
| `status` | 실행 또는 운영 상태 |
| `caseId` | 관련 Case ID, 해당 없으면 `null` |
| `evaluationRunId` | 관련 EvaluationRun ID, 해당 없으면 `null` |
| `timestamp` | Event 또는 상태 발생 시각, 해당 없으면 `null` |
| `metadataRef` | 상세 조회용 Reference |
| `detailAvailability` | 사용자의 권한과 데이터 보존 상태에 따른 상세 조회 가능 여부 |

권장 Node Type은 다음과 같다.

- `SERVICE`
- `AGENT`
- `TOOL`
- `POLICY`
- `EVENT`
- `CASE`
- `EVALUATION_RUN`
- `AGENT_RUN`
- `DECISION_ENVELOPE`
- `CONSEQUENCE_ENVELOPE`
- `EVIDENCE_FINDING`
- `HUMAN_TASK`
- `FINAL_DECISION`
- `EXTERNAL_SYSTEM`

## Graph Link 개념

최소 필드는 다음과 같다.

| 필드 | 의미 |
| --- | --- |
| `id` | Graph 내부에서 안정적인 Link 식별자 |
| `source` | Source Node ID |
| `target` | Target Node ID |
| `linkType` | 관계 유형 |
| `label` | 화면 표시용 짧은 관계 이름 |
| `eventType` | 관련 Event Type, 해당 없으면 `null` |
| `correlationId` | Case 또는 업무 흐름 상관관계 ID |
| `causationId` | 원인 Event ID |
| `timestamp` | 관계 발생 시각 |
| `status` | 성공, 실패, 생략, 차단 등 표시 상태 |
| `animated` | 선택·재생 중인 관계에 한해 Animation 적용 여부 |

## 표시 금지 데이터

Graph Node, Link, Tooltip, Side Panel 기본 Payload에는 다음 데이터를 포함하지 않는다.

- 주민등록번호
- 전체 계좌번호
- 원문 Prompt
- LLM 전체 응답
- 문서 원문
- 전체 금융 Snapshot
- Secret
- Access Token
- Raw Tool Credential

그래프에는 마스킹 ID, Reason Code, Version, 상태와 Reference만 표시한다. Reference를 클릭해 상세 데이터에 접근하는 경우에도 RBAC, 목적 제한, Audit Event가 필요하다.

## Source별 사용 원칙

| Source | 사용 범위 | 금지 |
| --- | --- | --- |
| Versioned Topology Manifest | Architecture Graph 렌더링 | 운영 DB 직접 조회로 대체 |
| Audit Read Model | Execution Graph 렌더링 | Web이 Kafka 또는 서비스 DB에서 직접 조합 |
| Governance Metadata | Agent Registry, EvaluationRun Reference | Agent 결론을 정책 판단으로 대체 |
| Contracts DTO | Graph API와 Payload 검증 | Docs 설명 모델을 실행 Schema로 간주 |

## Timeline과의 관계

Execution Graph는 Timeline을 대체하지 않는다. Timeline은 정확한 시간 순서와 접근성 대체 경로를 제공하고, Graph는 선택된 EvaluationRun의 인과관계 탐색을 보조한다. Graph와 Timeline이 의미상 불일치하면 Graph Read Model Drift로 기록하고 Release Readiness에서 검증해야 한다.
