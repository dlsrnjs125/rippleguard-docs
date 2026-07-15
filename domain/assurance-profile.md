# Assurance Profile

## 목적

결정론적 Preflight, Decision과 Consequence 분석, Evidence, Legacy Control Coverage, 실행 상태를 정책 엔진이 결정론적으로 평가할 수 있는 입력으로 종합한다.

| 구분 | 내용 |
| --- | --- |
| 핵심 입력 | Deterministic Preflight, Loan Decision Envelope, RippleGuard Consequence Envelope, Evidence & Control Findings, Legacy Control Coverage, System·Agent Health, OPA Policy Input Version |
| 핵심 필드 | `caseId`, input references, consequence risk dimensions, evidence completeness, control coverage, health status, hard-block reasons, required verifications, `COMPLETE`/`INCOMPLETE`/`VIOLATED` 상태, policy input version, provenance |
| 생성 주체 | Governance Service가 Preflight, Decision, Consequence, Evidence, Control Coverage, System Health를 각각 검증·종합하여 생성 |
| 소비 주체 | OPA, Governance Service, Audit & Replay Service, Governance Console |

어느 한 Agent 결과만으로 Assurance Profile을 생성하지 않는다. 일부 입력이 없거나 Schema·Evidence·Health 검증에 실패하면 누락을 명시하고 자동화 범위를 축소한다. Agent는 자료를 제안할 수 있지만 최종 Assurance 상태는 Governance Service가 확정한다. 실행 경로는 [Assurance Routing](../governance/assurance-routing.md)을 따르며, 필드명은 설명용이고 실행 가능한 최종 JSON Schema의 Source of Truth는 `rippleguard-contracts`다.
