# Data Flow

```mermaid
sequenceDiagram
  participant F as FDS Simulator
  participant W as Web
  participant L as Loan Service
  participant K as Kafka
  participant G as Governance Service
  participant A as Agent Runtime
  participant P as OPA
  participant R as Audit & Replay
  F->>L: 목적·범위 메타데이터가 포함된 위험신호 Snapshot
  W->>L: 대출 신청 (REST)
  L->>K: loan.application.submitted.v1
  K->>G: 신청 Event
  G->>G: Deterministic Preflight와 실행 계획 결정
  G->>K: agent.evaluation.requested.v1
  K->>A: Loan Decision Agent 실행 요청
  A->>K: Decision Envelope
  K->>G: Decision Envelope 검증
  G->>K: 전문 Agent 실행 계획
  par 독립 Consequence 분석
    K->>A: RippleGuard Agent + Decision/Snapshot
    A->>K: Consequence Envelope
  and 독립 Evidence·Control 검증
    K->>A: Evidence & Control Agent + Decision/Snapshot
    A->>K: Evidence & Control Findings
  end
  K->>G: 독립 결과 집계
  G->>G: Assurance Profile 생성
  G->>P: Versioned Policy Input (동기)
  P-->>G: Hard Block / Additional Verification / Safe Automation
  G->>K: Evidence 요청 또는 loan.decision.commanded.v1
  K->>L: 서비스 소유 상태 전이
  L->>K: 최종 상태 Event
  K->>R: 전 과정 Trace
```

Governance Service가 어떤 전문 Agent가 필요한지와 실행 순서를 결정하고, Agent Runtime은 그 계획을 실행한다. Agent Runtime이 호출 필요성을 정책적으로 확대하거나 서로의 결론을 입력으로 연결하지 않는다.

사용자 명령과 즉시 조회, OPA 정책 평가는 REST 또는 동기 API를 사용한다. 서비스 간 상태 변경, 장시간 Agent 실행, 감사 기록은 Kafka Event를 사용한다. Kafka 전달은 중복과 지연을 전제로 멱등 처리하며, 서비스별 상태 연결은 [Lifecycle Event Map](../domain/loan-case-lifecycle.md)에 정의한다. Event Schema의 원본은 `rippleguard-contracts`다.
