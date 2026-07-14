# Data Ownership

| 소유자 | 저장소 | 소유 범위 |
| --- | --- | --- |
| Loan Service | `loan_db` | 신청, 금융 Snapshot, 최종 대출 상태 |
| Governance Service | `governance_db` | Decision Case, Assurance, Agent 운영 상태, 정책 결과 |
| Agent Runtime | `agent_db` 또는 Checkpoint/RAG 저장소 | Agent 실행 checkpoint와 검색용 파생 데이터; 업무 원장은 아님 |
| Audit & Replay Service | `audit_db` | Append-only Trace, 감사 이벤트, Replay 및 Version Diff |
| MinIO | 문서 원본 객체 | 업로드 원본과 Evidence 객체; Metadata 소유자는 관련 서비스 |

## 금지 원칙

- 다른 서비스 DB에 직접 접근하지 않는다.
- Entity와 Repository 구현을 Repository 간 공유하지 않는다.
- Audit & Replay Service는 운영 데이터를 수정하지 않는다.
- Agent Runtime은 업무 DB에 직접 접근하거나 이를 원장으로 소유하지 않는다.
- 필요한 데이터는 계약된 API, Event 또는 목적이 제한된 Snapshot으로 전달한다.
