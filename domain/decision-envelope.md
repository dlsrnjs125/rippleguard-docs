# Decision Envelope

## 목적

Loan Decision Agent의 제안을 단순 점수가 아니라 사용 목적, 입력 Snapshot, 근거, 실행 버전과 함께 전달해 후속 Governance 검증이 가능하게 한다.

| 구분 | 내용 |
| --- | --- |
| 핵심 필드 | `caseId`, 제안 결과, 위험·상환 평가, 사용 데이터 목적, input snapshot reference, evidence references, agent/model/prompt/tool versions, 생성 시각 |
| 생성 주체 | Agent Runtime의 Loan Decision Agent |
| 소비 주체 | Governance Service, 조건부 RippleGuard Agent, Audit & Replay Service |

필드명은 설명용이다. 실행 가능한 최종 JSON Schema의 Source of Truth는 `rippleguard-contracts`이며 이 문서의 예시는 계약 원본이 아니다.
