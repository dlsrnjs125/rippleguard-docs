# Data Ownership

| 소유자 | 저장소 | 소유 범위 |
| --- | --- | --- |
| Loan Service | `loan_db` | 신청, 금융 Snapshot, 최종 대출 상태, 제출 문서 등록 Metadata(`documentId`, `caseId`, `objectKey`, 업로드 상태, 문서 유형) |
| Governance Service | `governance_db` | Decision Case, Evaluation Run, Assurance, 정책 결과, Schema 검증을 통과해 판단에 채택된 Versioned Evidence Finding Snapshot과 사용 Decision·Evidence·Agent/Prompt/Tool Version Reference |
| Agent Runtime | `agent_db` 또는 Checkpoint/RAG 저장소 | Agent checkpoint, 추출 Chunk, Embedding, 문서 파싱 중간 결과, 내부 Tool 결과와 채택 전 Finding 후보; 업무·판단 원장은 아님 |
| Audit & Replay Service | `audit_db` | Finding 생성·검증·사용 Event의 읽기 전용 Projection, Append-only Trace, Replay 및 Version Diff |
| MinIO | 문서·대용량 Artifact 객체 | 사용자 제출 원본과 선택적 파싱 원본 Artifact; 업무 상태와 Evidence 판단 결과를 소유하지 않음 |

## 금지 원칙

- 다른 서비스 DB에 직접 접근하지 않는다.
- Entity와 Repository 구현을 Repository 간 공유하지 않는다.
- Audit & Replay Service는 운영 데이터를 수정하지 않는다.
- Agent Runtime은 업무 DB에 직접 접근하거나 이를 원장으로 소유하지 않는다.
- 필요한 데이터는 계약된 API, Event 또는 목적이 제한된 Snapshot으로 전달한다.
- Loan Service의 문서 Metadata와 MinIO 원본, Agent Runtime 파생 데이터는 같은 `documentId`와 Versioned Reference로 연결하되 서로의 저장소를 직접 갱신하지 않는다.
- Governance Service가 채택한 Finding Snapshot은 해당 Evaluation Run의 불변 입력으로 보존한다. Agent 재파싱이나 Version 변경은 과거 Snapshot을 덮어쓰지 않고 새 Finding Version과 새 Run을 만든다.
- Evidence Finding 자체는 PostgreSQL의 Versioned Record로 관리하고, 큰 파싱 Artifact만 MinIO Object Reference로 연결한다.
