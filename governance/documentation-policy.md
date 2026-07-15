# Documentation Policy

| 내용 | Source of Truth |
| --- | --- |
| 전체 Phase 순서·의존성·범위, 시스템 관점, ADR, 통합·검증 Baseline | `rippleguard-docs` |
| 서비스 세부 설계·구현 계획·내부 Troubleshooting과 로컬 운영법 | 각 서비스 Repository |
| REST API, Kafka Event, Agent Output, Policy Input Schema 원본 | `rippleguard-contracts` |
| 실행·배포·통합 환경 설정 | `rippleguard-infra` |

Root README는 진입점과 탐색 정보만 유지한다. 중앙 Docs는 클래스, 내부 Graph, Prompt, Validator, Migration 구현을 복사하지 않고 소유 Repository의 문서와 검증 Commit을 연결한다. 변경 시 영향 Phase·Repository·계약·ADR을 함께 식별하고 확인하지 않은 URL, SHA, PR, 버전은 추측하지 않는다.

프로젝트 전체 위험과 Cross-Repo Blocker는 중앙 Risk Register와 Troubleshooting Index에서 관리한다. 단일 서비스 문제의 상세 원인·시도·해결은 해당 Repository에 두며, 여러 Repository의 계약·통합 실패만 중앙에 별도 요약한다.
