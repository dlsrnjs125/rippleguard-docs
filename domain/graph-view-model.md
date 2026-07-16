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
| `metadataRef` | 상세 조회용 opaque application reference |
| `detailAvailability` | 사용자의 권한과 데이터 보존 상태에 따른 상세 조회 가능 여부 Enum |

권장 Node Type은 다음과 같다.

- `SERVICE`
- `AGENT`
- `TOOL`
- `POLICY`
- `POLICY_DECISION`
- `ROUTING_DECISION`
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

`POLICY`는 정적 정책 정의와 `policyVersion`, `bundleVersion`을 표현한다. `POLICY_DECISION`은 특정 EvaluationRun의 정책 실행 결과와 `allow`, `deny`, `route`, `reasonCodes`, `evaluatedAt`을 표현한다. 필요하면 최종 Routing 결과를 `ROUTING_DECISION`으로 분리한다.

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
| `linkOrigin` | Edge의 출처와 증거 성격 |
| `evidenceRefs` | Edge를 뒷받침하는 Event, Run, Policy Decision Reference |
| `causalityStatus` | 인과관계 검증 상태 |
| `timestamp` | 관계 발생 시각 |
| `status` | 성공, 실패, 생략, 차단 등 표시 상태 |
| `animationEligible` | Event 흐름 표현에 Animation을 적용할 수 있는지 여부 |

`linkOrigin` 값은 다음 중 하나를 사용한다.

- `OBSERVED_EVENT_CAUSATION`: `causationId`로 증명된 실제 Event 인과관계
- `OBSERVED_REFERENCE`: `evaluationRunId`, `agentRunId`, `policyDecisionId` 등 Reference로 관측된 관계
- `DERIVED_RELATION`: Envelope 생성, 정책 검증, UI 그룹 연결처럼 Read Model에서 파생한 관계
- `STATIC_TOPOLOGY`: Architecture Graph의 정적 관계

`causalityStatus` 값은 다음 중 하나를 사용한다.

- `VERIFIED`: Event 또는 Reference로 검증됨
- `INFERRED`: 직접 causation은 아니지만 Read Model 규칙으로 추론됨
- `GAP`: 예상 관계의 Event 또는 Reference가 누락됨
- `CONFLICTED`: Timeline, Event, Reference가 서로 충돌함

`evidenceRefs`에는 직접 URL이나 원문 식별자를 넣지 않고 다음과 같은 application reference만 둔다.

- `sourceEventId`
- `targetEventId`
- `evaluationRunId`
- `agentRunId`
- `policyDecisionId`

시각 표현은 Edge 성격을 구분해야 한다.

- 실선: `OBSERVED_EVENT_CAUSATION`
- 점선: `OBSERVED_REFERENCE` 또는 `DERIVED_RELATION`
- 경고선: `GAP` 또는 `CONFLICTED`

`animated`는 서버 DTO 필드가 아니다. Web은 `animationEligible`, 선택 Link, Playback Cursor, 사용자 설정과 Reduced Motion 설정을 조합해 실제 Animation 여부를 계산한다.

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

그래프에는 마스킹 ID, Reason Code, Version, 상태와 Reference만 표시한다. `metadataRef`는 opaque application reference여야 하며 `s3://`, MinIO URL, Presigned URL, 원문 파일 경로를 포함하지 않는다. Graph API 응답에 Presigned URL을 포함하는 것은 금지한다.

상세 조회는 별도 Authorized API를 통해 수행하며 Case, Role, Purpose, Retention을 다시 검증한다. Reference enumeration을 막기 위해 순차 추측 가능한 직접 Object Key를 사용하지 않는다.

`detailAvailability`는 Boolean이 아니라 다음 Enum 중 하나다.

- `NONE`
- `SUMMARY_ONLY`
- `REFERENCE_ONLY`
- `AUTHORIZED_DETAIL`
- `EXPIRED`
- `REDACTED`

Reference를 클릭해 상세 데이터에 접근하는 경우에도 RBAC, 목적 제한, Audit Event가 필요하다.

## Source별 사용 원칙

| Source | 사용 범위 | 금지 |
| --- | --- | --- |
| Versioned Topology Manifest | Architecture Graph 렌더링 | 운영 DB 직접 조회로 대체하거나 Web 내부 정적 JSON으로 하드코딩 |
| Audit Read Model | Execution Graph 렌더링 | Web이 Kafka 또는 서비스 DB에서 직접 조합 |
| Governance Metadata | Agent Registry, EvaluationRun Reference | Agent 결론을 정책 판단으로 대체 |
| Contracts DTO | Graph API와 Payload 검증 | Docs 설명 모델을 실행 Schema로 간주 |

## Timeline과의 관계

Execution Graph는 Timeline을 대체하지 않는다. Timeline은 정확한 시간 순서와 접근성 대체 경로를 제공하고, Graph는 선택된 EvaluationRun의 인과관계 탐색을 보조한다. Graph와 Timeline이 의미상 불일치하면 Graph Read Model Drift로 기록하고 Release Readiness에서 검증해야 한다.
