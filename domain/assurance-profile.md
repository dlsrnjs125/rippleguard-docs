# Assurance Profile

## 목적

Decision 및 Consequence 분석, Evidence, Legacy Control 이행 여부를 정책 엔진이 결정론적으로 평가할 수 있는 입력으로 종합한다.

| 구분 | 내용 |
| --- | --- |
| 핵심 필드 | `caseId`, decision/consequence references, evidence completeness, required control checks, violations, `COMPLETE`/`INCOMPLETE`/`VIOLATED` 상태 후보, policy input version, provenance |
| 생성 주체 | Evidence & Control Agent의 구조화 결과를 검증·종합하는 Governance Service |
| 소비 주체 | OPA, Governance Service, Audit & Replay Service, Governance Console |

Agent는 자료를 제안할 수 있지만 최종 Assurance 상태는 Governance Service가 확정한다. 필드명은 설명용이며 실행 가능한 최종 JSON Schema의 Source of Truth는 `rippleguard-contracts`다.
