# Service Boundaries

| 서비스 | 책임과 소유 데이터 | 외부 제공 API/Event | 호출·구독 가능 경계 | 금지된 직접 접근 | 장애 시 안전 경로 |
| --- | --- | --- | --- | --- | --- |
| FDS Simulator / External Risk Signal Source | 합성 위험신호와 목적·범위 Metadata 제공 | 위험신호 Fixture 또는 API | Loan Service의 제한된 수집 경계 | 대출·Governance·Agent DB와 최종 결과 | 신호 누락·만료 시 해당 신호 사용 금지 |
| Loan Service | 대출 신청, 금융 Snapshot, 최종 대출 상태 | 신청 REST, 조회 REST, 신청 Event, 최종 처리 Command 소비 | Governance 결과 Event 소비 | `governance_db`, `agent_db`, `audit_db` | 신규 자동 결정 보류, 기존 상태 유지, 수동 심사 |
| Governance Service | Decision Case, Evaluation Run, Assurance, 채택된 Finding Snapshot, Agent 상태, 정책 실행 통제 | Case 조회·운영 REST, Agent 평가 요청, 대출 처리 Command | Loan Event, Agent 결과 Event, OPA | `loan_db`, Agent checkpoint, `audit_db` 수정 | Manual Safe Mode 또는 추가 검증으로 축소 |
| Agent Runtime | 등록 Agent 실행, Tool 호출, checkpoint/RAG | 평가 결과 Event, 실행 상태 | 평가 요청 Event, 목적 제한 API·Object Reference·필요 Chunk | 업무 DB, 전체 원문 기본 접근, 최종 상태 변경 API | 실패 결과 기록, 재시도 한도 후 사람 검증 |
| Audit & Replay Service | Trace, 감사로그, Replay, Version Diff | 감사 조회·Replay API | 관련 Event, MinIO 원본·파싱 Artifact Reference | 운영 서비스 데이터 수정 | 원 이벤트 보존, Replay 중단을 운영 결정과 격리 |
| Web | 신청자·심사자·Governance UI 상태 | 사용자 화면 | 역할별 공개 REST API | 모든 DB와 내부 Event 직접 접근 | 쓰기 차단 및 읽기 가능한 상태·오류 안내 |

모든 서비스 간 데이터 교환의 실행 가능한 계약 원본은 `rippleguard-contracts`에 둔다. 여기의 Event 이름은 경계 설명용이며 구현 Schema가 아니다.
