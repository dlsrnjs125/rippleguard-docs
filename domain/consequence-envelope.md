# Consequence Envelope

## 목적

대출 제안이 원 데이터의 목적과 의미, 판단 범위를 어떻게 변형했는지 분석하고 Cross-Purpose Data Reuse 또는 Scope Escalation 가능성을 명시한다.

| 구분 | 내용 |
| --- | --- |
| 핵심 필드 | `caseId`, 대상 decision reference, detected risks, source purpose, inferred purpose, original scope, expanded scope, semantic transformations, evidence references, run/version metadata |
| 생성 주체 | Agent Runtime의 RippleGuard Agent |
| 소비 주체 | Governance Service, Evidence & Control Agent, Audit & Replay Service |

필드명은 설명용이다. 실행 가능한 최종 JSON Schema의 Source of Truth는 `rippleguard-contracts`이며 이 문서의 예시는 계약 원본이 아니다.
