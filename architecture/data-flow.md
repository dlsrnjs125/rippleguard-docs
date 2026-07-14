# Data Flow

```mermaid
sequenceDiagram
  participant W as Web
  participant L as Loan Service
  participant K as Kafka
  participant G as Governance Service
  participant A as Agent Runtime
  participant P as OPA
  participant R as Audit & Replay
  W->>L: 대출 신청 (REST)
  L->>K: loan.application.submitted.v1
  K->>G: 신청 Event
  G->>K: agent.evaluation.requested.v1
  K->>A: 평가 요청
  A->>K: Loan Proposal / agent.evaluation.completed.v1
  K->>G: Agent 결과
  Note over G,A: 조건부 RippleGuard / Evidence Agent 실행
  G->>P: Assurance Profile 정책 평가 (동기)
  P-->>G: 자동 처리 / 추가 검증 / 차단
  G->>K: loan.decision.commanded.v1
  K->>L: 최종 처리 명령
  L->>K: 최종 상태 Event
  K->>R: 전 과정 Trace
```

사용자 명령, 즉시 조회, 정책처럼 즉시 응답이 필요한 검증은 REST 또는 동기 API를 사용한다. 서비스 간 상태 변경, 장시간 Agent 실행, 감사 기록은 Kafka Event로 결합도를 낮춘다. Kafka 전달은 중복과 지연을 전제로 멱등 처리하며, Event Schema의 원본은 `rippleguard-contracts`에 둔다.
