# Cross-Repo Versioning

각 Phase 종료 시 호환성이 함께 검증된 Repository 조합을 해당 Phase의 `cross-repo-baseline.md`에 기록한다.

| 항목 | 기록 내용 |
| --- | --- |
| Repository | Repository 이름 |
| Branch | Phase 작업 브랜치 |
| PR | 관련 PR URL 또는 번호 |
| Candidate Baseline | 리뷰 중에는 `PR Head`; 병합 전 검증 후보 |
| Published Baseline | 병합 뒤 고정한 Merge Commit SHA 또는 Phase Tag |
| Docker Image | digest 또는 불변 이미지 태그 |
| Contract Version | 사용한 계약 버전 |
| DB Migration | 적용된 Migration 식별자 |
| Verification | 테스트·검증 결과 링크 |
| Status | `NOT_STARTED`, `IN_PROGRESS`, `IN_REVIEW`, `VERIFIED`, `BLOCKED` |

Branch 이름만으로 Baseline을 고정하지 않으며 변경 가능한 `latest` 태그도 기준으로 쓰지 않는다. PR 중에는 자기 문서의 최신 Commit을 반복 기록하지 않고 Candidate를 `PR Head`로 표현한다. 병합 뒤 별도 Metadata Update에서 Merge SHA 또는 승인 Tag를 Published Baseline으로 고정한다. Baseline 변경은 영향을 받는 Repository의 호환성 검증 후 남기며 검증 전 값은 `TBD`다.
