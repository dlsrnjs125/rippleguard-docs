# Phase 0 Verification

## 검증 목표

Phase 0 문서가 후속 멀티레포 개발의 일관된 기준과 탐색 경로를 제공하는지 확인한다.

| 검증 항목 | 결과 | 근거·비고 |
| --- | --- | --- |
| 문서 링크가 깨지지 않는가 | PASS | 모든 Markdown 로컬 링크 대상 존재 확인 |
| Repository 책임이 중복되지 않는가 | REVIEWED | Repository Strategy와 Service Boundaries 대조 |
| 데이터 소유권이 충돌하지 않는가 | REVIEWED | 서비스별 DB, MinIO 원본, 금지 원칙 명시 |
| Loan과 Governance 상태 소유권이 분리되는가 | REVIEWED | 두 상태 머신과 Event 연결표 대조 |
| Agent 전문 검증이 독립적인가 | REVIEWED | 공통 Decision 입력 후 독립 실행, Governance 집계 명시 |
| Hard Block이 평균 Score로 상쇄되지 않는가 | REVIEWED | Risk Model과 Assurance Routing 대조 |
| Hard Block 원인 제거 후 새 Run 경로가 명확한가 | REVIEWED | 차단 Run 불변, Case 재산출, 새 Snapshot·Evaluation Run 대조 |
| 결정론적 검사와 Agent 의미 검사가 분리되는가 | REVIEWED | Agent Governance 담당표 대조 |
| 조건부 Agent Trigger가 위험 Case를 우회하지 않는가 | REVIEWED | Trigger Matrix, 생략 Trace와 Sample Audit 기준 대조 |
| Phase 6 Trace 의존성이 Phase 7을 역참조하지 않는가 | REVIEWED | Phase 1~2 Audit Foundation과 Phase 7 확장 경계 대조 |
| FDS Demo와 Test 입력 경로가 구분되는가 | REVIEWED | REST Container와 고정 JSON Fixture 기준 대조 |
| 문서 원본·Metadata·파생 Finding 소유권이 명확한가 | REVIEWED | Data Ownership 서비스별 항목 대조 |
| 채택된 Evidence Finding이 과거 Run에서 재현되는가 | REVIEWED | Governance Versioned Snapshot과 Audit Event Projection 경계 확인 |
| MinIO가 Evidence 판단 상태를 소유하지 않는가 | REVIEWED | 원본·선택적 파싱 Artifact와 PostgreSQL Finding Record 구분 |
| Human Verification이 실제 진위 확인을 주장하지 않는가 | REVIEWED | 합성 Metadata·기간·명의·형식 검토로 범위 제한 |
| Sample Audit Escape 후속 흐름이 후속 Phase에 계획됐는가 | REVIEWED | Phase 7 영향 Run 검색·Replay와 Phase 8 축소·신규 Run Drill 확인 |
| Holdout 평가 누수와 Parser 오류 혼입을 통제하는가 | REVIEWED | Dataset Partition, 접근 이력과 분리 Metric 대조 |
| PR 중 Baseline이 자기 SHA를 반복 기록하지 않는가 | PASS | Candidate `PR Head`, 병합 후 Published SHA·Tag 규칙 확인 |
| 민감정보와 문서 Prompt Injection 통제가 있는가 | REVIEWED | 최소 Context, 마스킹, Tool 권한 불변 원칙 명시 |
| Docs와 Contract Source of Truth가 구분되는가 | REVIEWED | README, Documentation Policy, Envelope에 명시 |
| Phase 추적 절차가 명확한가 | REVIEWED | Phase README와 Versioning Policy에 절차 정의 |
| Phase 1~10 계획과 의존성이 연결되는가 | REVIEWED | Phase별 README·Plan, Dependency Map과 Status 대조 |
| 미래 Phase에 실행용 빈 문서가 과도하게 생성되지 않았는가 | PASS | Phase 1~10에 계획용 README·Plan만 존재 확인 |
| 전역 위험·Roadmap 변경·Release 완료 기준이 추적되는가 | REVIEWED | Risk Register, Change Log, Release Readiness 대조 |
| Troubleshooting Template이 사용 가능한가 | REVIEWED | 필수 기록 항목 포함 |
| README에서 주요 문서로 이동할 수 있는가 | PASS | Root README의 주요 문서 링크 대상 존재 확인 |
| Contract Schema 검증 | PASS | `rippleguard-contracts` main `a44f39d819fff5fe79db27e236a42e2a861a8b5e`; `make validate` 결과: 16 schemas, 30 valid examples, 54 intentional invalid examples, 8 scenarios, 1 compatibility check |
| Valid·Invalid Example 검증 | PASS | `rippleguard-contracts` `make validate`에 포함 |
| Contracts CI 상태 | PASS | `rippleguard-contracts` PR #2 head `39b22e40ae10a9ec3678cfac509a7fe6b747eaa4`; GitHub Actions `Contract validation` run #8 success |
| Compose config 검증 | PASS | `rippleguard-infra` main `22db5e4fcc435d2492ad10bf33c18618ecc46282`; `make validate-static` PASS |
| Contracts와 Infra Topic 일치 | PASS | `rippleguard-infra` `make validate-contract-baseline` PASS; pinned contracts commit checkout 후 schema eventType과 topic 비교 |
| Kafka Topic 검증 | PASS | `make platform-up` topic init 성공, `make platform-check` `Kafka topics verified` |
| Loan/Governance DB 연결 | PASS | `make platform-check`; 양 DB health, 연결, container/volume/hostname/system_identifier 분리와 credential cross-check 통과 |
| OPA Health | PASS | `make platform-check`; OPA container health와 `/health` endpoint 확인 |
| MinIO Health | PASS | `make platform-check`; MinIO health와 `rippleguard-documents` bucket 확인 |
| Infra CI 상태 | PASS | `rippleguard-infra` PR #1 head `06e6f30fb20bdc121b163699acdb33330cafd0b8`; GitHub Actions `Infra validation` run #5 success; commands: `make validate-static`, `make validate-contract-baseline` |
| Docs finalization baseline | PASS | Docs PR #4 merged; merge commit `d729e21c62a2372ea044c6f68fe827595b266483` fixed as Phase 0 docs published baseline |

추가 정적 검사에서 빈 파일과 `git diff --check` 오류가 없음을 확인했다. `REVIEWED`는 문서 의미의 정적 검토 결과다. Contracts, Infra와 Docs finalization baseline이 모두 main 병합 commit으로 고정되어 Phase 0 상태는 `VERIFIED`다.
