# Phase 1 Verification

## 검증 목표

AI 없는 Core MSA 최소 수직 흐름이 계약, 상태 소유권, 멱등성, 독립 DB와 Audit Foundation 기준을 만족하는지 검증한다.

## 환경

| 항목 | 값 |
| --- | --- |
| 실행 시각 | TBD |
| 관련 Commit | TBD |
| 환경·도구 버전 | TBD |

## 검증 항목

| 항목 | 명령·방법 | 기대 결과 | 실행 결과 | 상태 |
| --- | --- | --- | --- | --- |
| Loan 신청 REST 동작 | TBD | 신청 요청이 검증되고 신청 ID가 반환됨 | NOT_RUN | PENDING |
| Versioned Snapshot 저장 | TBD | 신청 입력과 FDS Snapshot이 versioned record로 저장됨 | NOT_RUN | PENDING |
| Application Event 발행 | TBD | `loan.application.submitted.v1`이 outbox를 통해 발행됨 | NOT_RUN | PENDING |
| Decision Case 생성 | TBD | Governance Service가 신청 Event로 Decision Case를 1회 생성함 | NOT_RUN | PENDING |
| Mock Evaluation 재현 | TBD | 고정 Mock Evaluator가 동일 입력에 동일 Decision Envelope를 생성함 | NOT_RUN | PENDING |
| Decision Command 적용 | TBD | Governance Command를 Loan Service가 검증 후 적용함 | NOT_RUN | PENDING |
| 중복 Event 멱등 처리 | TBD | 중복 Event가 중복 Case 또는 중복 최종 결정을 만들지 않음 | NOT_RUN | PENDING |
| 최종 상태 1회 반영 | TBD | Loan 최종 상태가 한 번만 전이됨 | NOT_RUN | PENDING |
| 최소 Case Timeline 조회 | TBD | 신청부터 최종 상태까지 최소 Timeline 조회 가능 | NOT_RUN | PENDING |
| 독립 DB 소유권 | TBD | Loan/Governance/Audit DB 직접 교차 접근 없음 | NOT_RUN | PENDING |
| Docker Compose E2E | TBD | 빈 환경에서 최소 수직 흐름 재현 | NOT_RUN | PENDING |
| Contracts·Image·Migration Baseline | TBD | 병합 commit, contract version, image tag, migration이 고정됨 | NOT_RUN | PENDING |

## 실패 항목

아직 검증을 실행하지 않았다. 실패가 발생하면 [Troubleshooting](troubleshooting/README.md)에 연결한다.

## 잔여 위험

- Phase 1 구현 Repository는 아직 `NOT_STARTED`다.
- REST/OpenAPI와 DB Migration은 Phase 1 각 Repository 작업에서 확정된다.
- Phase 1 시작은 구현 완료를 의미하지 않는다.
