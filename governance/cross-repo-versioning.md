# Cross-Repo Versioning

각 Phase 종료 시 호환성이 함께 검증된 Repository 조합을 해당 Phase의 `cross-repo-baseline.md`에 기록한다.

| 항목 | 기록 내용 |
| --- | --- |
| Repository | Repository 이름 |
| Branch | Phase 작업 브랜치 |
| Commit | 검증된 전체 Commit SHA |
| PR | 관련 PR URL 또는 번호 |
| Docker Image | digest 또는 불변 이미지 태그 |
| Contract Version | 사용한 계약 버전 |
| DB Migration | 적용된 Migration 식별자 |
| Verification | 테스트·검증 결과 링크 |
| Status | `NOT_STARTED`, `IN_PROGRESS`, `VERIFIED`, `BLOCKED` |

Branch 이름만으로 Baseline을 고정하지 않으며 변경 가능한 `latest` 태그도 기준으로 쓰지 않는다. Baseline 변경은 영향을 받는 Repository의 호환성 검증 후 문서 변경으로 남긴다. 검증 전 값은 `TBD`이고 추측으로 채우지 않는다.
