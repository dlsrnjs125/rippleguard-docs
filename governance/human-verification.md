# Human Verification

사람 검토는 전체 대출심사를 비정형적으로 다시 수행하거나 정책 위반을 Override하는 절차가 아니다. 시스템이 확정하지 못한 제한된 사실과 증빙만 확인한다.

## 허용 범위

- 문서와 정형 입력의 불일치 여부 확인
- 합성 발급정보와 문서 Metadata의 일관성 확인
- 신청자와 문서 명의의 연결 확인
- 문서 기간과 심사 기준일의 일치 여부 확인
- 제공된 서명·발급번호의 형식 검증
- 누락 Evidence 요청과 수신 결과 분류
- `SUSPECTED` 상태를 그대로 유지하거나 승인된 출처로 확인된 사실을 별도 필드에 기록

## 금지 범위

- 금지된 데이터 목적 사용 또는 Legacy Control 위반 Override
- Agent Output, Assurance 상태, OPA 결과 직접 편집
- 근거 없이 FDS 추정을 확정 사실로 변경
- 최종 대출 상태 직접 변경 또는 비정형 메모만으로 자동화 재개
- 실제 발급기관 조회, 전자서명 검증 또는 원본 문서 진위 확인을 수행했다고 주장

실제 문서 진위 검증은 발급기관 API, 전자서명·인증서 검증과 승인된 신뢰 인프라가 필요한 후속 범위다. MVP의 사람 확인은 제공된 합성 정보와 Metadata 사이의 일관성 검토로 제한한다.

## 재평가 절차

1. Governance Service가 확인할 사실, 허용 Evidence, 만료 시각을 구조화해 요청한다.
2. 권한 있는 Loan Reviewer가 사실과 Evidence Reference를 정형 결과로 제출한다.
3. Loan Service가 필요한 Snapshot을 새 Version으로 저장하고 Event를 발행한다.
4. Governance Service가 Deterministic Preflight, 필요한 Agent, Assurance Profile과 OPA 정책을 새 Run으로 재실행한다.
5. 기존 Run과 결과를 덮어쓰지 않고 Version Diff와 Trace를 연결한다.

사람 확인에도 최소 권한, 이중 확인이 필요한 고위험 변경, 사유와 provenance 기록을 적용한다.
