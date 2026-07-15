# Release Readiness

Phase 10에서 전체 프로젝트가 재현 가능한 최종 상태인지 판정한다. 미검증 항목을 완료로 추정하지 않는다.

| 영역 | 완료 기준 | Evidence | 상태 |
| --- | --- | --- | --- |
| Repository | 서비스별 검증 Commit과 병합 PR 고정 | TBD | PENDING |
| Image | 모든 Docker Image를 불변 Tag 또는 Digest로 고정 | TBD | PENDING |
| Contract | Producer·Consumer 호환 Contract Version 고정 | TBD | PENDING |
| Migration | 서비스별 DB Migration 순서와 결과 고정 | TBD | PENDING |
| Evaluation | Golden Case 평가와 한계 보고 완료 | TBD | PENDING |
| Reliability | 장애 Drill과 Degraded Mode 검증 완료 | TBD | PENDING |
| Audit | FDS 입력부터 최종 상태까지 Trace·Replay 검증 | TBD | PENDING |
| Security | 역할·권한, 민감정보, Prompt Injection Test 완료 | TBD | PENDING |
| Documentation | Markdown Link와 실행 절차 검증 | TBD | PENDING |
| Architecture | 최종 문서가 실제 구현과 일치 | TBD | PENDING |
| Limitations | 남은 위험·제약·후속 개선 방향 기록 | TBD | PENDING |

상태는 `PENDING`, `PASS`, `BLOCKED`, `ACCEPTED_RISK` 중 하나다. 필수 항목이 `PENDING` 또는 `BLOCKED`면 Release Ready로 선언할 수 없다. `ACCEPTED_RISK`에는 승인 주체와 근거가 필요하다.
