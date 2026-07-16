# Security and Privacy

## 데이터 분류와 최소 Context

신청자 식별정보, 계좌정보, 소득·부채·연체, 플랫폼 정산, FDS 신호와 제출 문서는 개인정보 또는 금융정보로 분류한다. 각 Agent에는 목적 수행에 필요한 최소 필드와 Chunk만 전달하며 전체 신청자 Profile을 기본 Context로 제공하지 않는다.

- 계좌번호와 직접 식별자는 전달·Trace 전에 마스킹 또는 토큰화한다.
- Agent Runtime은 업무 DB에 직접 접근하지 않는다.
- MinIO 문서는 Case, 목적, 역할, 유효기간이 제한된 읽기 권한으로 접근한다.
- 원문 대신 Object Reference와 필요한 Chunk를 전달하고, 원문 접근은 별도 감사 Event로 남긴다.
- Agent Trace와 Prompt에는 원문, Secret, 전체 계좌번호, 불필요한 민감정보를 저장하지 않는다.
- Applicant, Loan Reviewer, AI Governance Operator, AI Auditor, 서비스 계정별 RBAC를 적용한다.
- Secret은 코드·문서·Image에 저장하지 않고 환경별 Secret Store 또는 주입 방식으로 관리하며 회전 가능해야 한다.

보존기간, 암호화, 마스킹과 삭제 정책은 데이터 분류별로 구성하고 Audit Reference가 원문 복사를 유발하지 않게 한다.

## Graph Visualization 보안 경계

Graph Node, Link, Tooltip과 Side Panel 기본 Payload에는 주민등록번호, 전체 계좌번호, 원문 Prompt, LLM 전체 응답, 문서 원문, 전체 금융 Snapshot, Secret, Access Token, Raw Tool Credential을 포함하지 않는다.

Graph에는 마스킹 ID, Reason Code, Version, 상태와 Reference만 표시한다. `metadataRef`는 opaque application reference이며 Graph API 응답에 Presigned URL, MinIO URL, 원문 Object Key를 포함하지 않는다. 상세 조회는 별도 Authorized API에서 Case, Role, Purpose, Retention을 재검증하고 Audit Event를 남긴다. 학습 데이터 전체, 모든 RAG Chunk, Embedding 관계, 모든 로그 Event를 MVP Graph에 펼치지 않는다.

Graph 민감정보 차단의 1차 책임은 Audit & Replay Service의 Graph Read Model과 Contracts Schema에 있다. Web은 허용된 Summary만 표시하고 DOM·Canvas Tooltip에 민감정보가 노출되지 않게 해야 하지만, 서버가 원문을 보낸 뒤 Web에서 숨기는 구조는 허용하지 않는다.

## 문서 Prompt Injection

업로드 문서는 신뢰할 수 없는 **데이터**이며 Agent 명령이 아니다.

- 문서 안의 지시문은 System Prompt, 실행 계획, Tool 권한 또는 정책을 변경할 수 없다.
- System·Developer 지시와 Tool allowlist를 문서 Content보다 우선한다.
- 문서 Chunk는 명확한 데이터 경계와 source reference로 감싸 전달한다.
- 문서가 외부 URL 호출, Secret 공개, 추가 Tool 사용 또는 다른 문서 접근을 요구해도 실행하지 않는다.
- 의심스러운 지시문은 Evidence와 분리해 `PROMPT_INJECTION_SUSPECTED`로 기록하고 자동화를 축소한다.
- 공격 문구가 포함된 Golden Case로 Tool 권한 유지와 민감정보 비노출을 회귀 검증한다.
