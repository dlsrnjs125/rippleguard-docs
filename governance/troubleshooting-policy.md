# Troubleshooting Policy

트러블슈팅은 `TS-P00-001-short-description.md`, `TS-P01-001-short-description.md` 형식을 사용한다. 단일 Repository의 상세 원인·시도·해결은 해당 Repository `docs/troubleshooting/`에 기록하고 중앙 Docs에는 요약과 확인된 링크만 둔다. 여러 Repository가 함께 실패한 계약·통합 문제는 중앙 Phase 디렉터리에 별도 요약한다.

각 기록에는 발생 일시, 관련 Phase, 영향받은 Repository와 서비스, 발생 Context, 증상, 영향 범위, 원인, 검토한 가설과 시도, 최종 해결, 검증 방법, 재발 방지, 관련 Commit·PR·ADR을 포함한다. 확인되지 않은 원인은 사실처럼 기록하지 않고 가설로 구분한다.

새 기록은 [Troubleshooting Template](../templates/troubleshooting-template.md)으로 작성하고, Phase README와 [전체 Index](../tracking/troubleshooting-index.md)에 링크한다. 민감정보, 인증정보, 원본 고객 데이터는 문서에 복사하지 않는다.
