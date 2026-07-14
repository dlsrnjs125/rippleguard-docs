# Documentation Policy

| 내용 | Source of Truth |
| --- | --- |
| 전체 시스템 관점, 도메인 기준, 아키텍처, ADR, Phase Baseline | `rippleguard-docs` |
| 서비스 세부 구현과 로컬 운영법 | 각 서비스 Repository |
| REST API, Kafka Event, Agent Output, Policy Input Schema 원본 | `rippleguard-contracts` |
| 실행·배포·통합 환경 설정 | `rippleguard-infra` |

Root README는 진입점과 탐색 정보만 유지한다. 설명을 중복 복사하지 않고 원본 문서에 링크하며, 변경 시 영향 Phase·Repository·계약·ADR을 함께 식별한다. 확인하지 않은 URL, SHA, PR, 버전은 추측하지 않고 `TBD` 또는 `NOT_STARTED`로 표시한다.
