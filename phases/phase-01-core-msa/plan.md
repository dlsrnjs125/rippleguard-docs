# Phase 1 Plan

- [x] `rippleguard-contracts` Phase 1 계약 PR 병합
- [x] 신청 REST와 Snapshot 계약 확정
- [x] Phase 0 상태·Event 계약 재사용 범위와 Consumer 적용 규칙 확정
- [x] Phase 0 Published Contract의 호환 변경 원칙과 major 변경 ADR 요구 기준 확인
- [x] `rippleguard-loan-service` 신청·Snapshot·Outbox PR 병합
- [x] Loan/Governance/Audit DB Migration 작성
- [x] Outbox·Inbox와 Consumer 멱등성 구현
- [x] `rippleguard-governance-service` Decision Case·Mock Evaluation PR 병합
- [x] Mock Agent·Assurance 흐름 구현
- [x] Decision Command와 최종 상태 반영 서비스 테스트
- [x] `rippleguard-audit-replay-service` 최소 Event 저장·Timeline PR 병합
- [x] `correlationId`·`causationId`와 원본 Event 저장 구현
- [x] 최소 Case Timeline 조회 API 구현
- [x] `rippleguard-infra` Docker Compose E2E 통합 PR 병합
- [ ] Docker Compose 최소 수직 흐름 실행
- [ ] Kafka 중복·지연·실패 시나리오의 로컬 통합 검증
- [ ] Phase 1 service image OCI provenance label 검증
- [ ] `rippleguard-docs` Phase 1 finalization PR 병합
- [ ] Verification과 Cross-Repo Baseline 고정

현재까지 완료된 항목은 병합된 main commit, GitHub Checks API, 로컬 계약/서비스 테스트로 확인한 범위만 체크했다. Docker Compose E2E와 image provenance 검증은 아직 완료되지 않았으므로 Phase 1은 `VERIFIED`가 아니다.
