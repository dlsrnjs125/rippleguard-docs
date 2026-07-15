# Change Management

1. API, Event, Agent Output 또는 Policy Input 변경은 `rippleguard-contracts`에서 먼저 제안하고 호환성 영향을 기록한다.
2. Consumer가 새 계약을 안전하게 처리하도록 배포한 뒤 Producer가 새 필드를 발행한다.
3. 호환되지 않는 Event 의미·구조 변경은 기존 Event를 변형하지 않고 새 버전을 사용한다.
4. 서비스 변경이 다른 Repository에 미치는 API, 데이터, 배포, 운영 영향을 PR과 Phase 문서에 기록한다.
5. 함께 검증된 조합이 바뀌면 Cross-Repo Baseline을 갱신한다.
6. Prompt, 모델, Tool 또는 정책 변경은 Golden Case Replay와 Shadow 검증 후 승인된 버전을 활성화한다.

아키텍처 경계 또는 장기 Trade-off가 달라지는 변경은 ADR을 추가하거나 기존 ADR을 Superseded 처리한다.
