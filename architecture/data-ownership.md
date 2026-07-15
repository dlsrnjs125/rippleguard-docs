# Data Ownership

| 소유자 | 저장소 | 소유 범위 |
| --- | --- | --- |
| Loan Service | `loan_db` | 신청, 금융 Snapshot, 최종 대출 상태, 제출 문서 등록 Metadata(`documentId`, `caseId`, `objectKey`, 업로드 상태, 문서 유형) |
| Governance Service | `governance_db` | Decision Case, Evaluation Run, Assurance, Agent 운영 상태, 정책 결과, 어떤 Finding이 어떤 Decision에 사용됐는지 Reference |
| Agent Runtime | `agent_db` 또는 Checkpoint/RAG 저장소 | Agent checkpoint, 추출 Chunk, Embedding, 문서 파싱 결과, Evidence Finding; 업무 원장은 아님 |
| Audit & Replay Service | `audit_db` | Append-only Trace, 감사 이벤트, Replay 및 Version Diff |
| MinIO | 문서 원본 객체 | 사용자 제출 원본 Object; 업무 Metadata나 판단 상태를 소유하지 않음 |

## 금지 원칙

- 다른 서비스 DB에 직접 접근하지 않는다.
- Entity와 Repository 구현을 Repository 간 공유하지 않는다.
- Audit & Replay Service는 운영 데이터를 수정하지 않는다.
- Agent Runtime은 업무 DB에 직접 접근하거나 이를 원장으로 소유하지 않는다.
- 필요한 데이터는 계약된 API, Event 또는 목적이 제한된 Snapshot으로 전달한다.
- Loan Service의 문서 Metadata와 MinIO 원본, Agent Runtime 파생 데이터는 같은 `documentId`와 Versioned Reference로 연결하되 서로의 저장소를 직접 갱신하지 않는다.
