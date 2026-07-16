# Phase 0 — Foundation & Architecture Baseline

- Phase: 0
- 상태: `IN_PROGRESS`
- 선행 Phase: 없음
- 후속 Phase: Phase 1

## 목표와 필요성

프로젝트 문제, MVP 범위, 서비스·데이터 소유권, Agent 권한, 기술 스택과 계약 기반을 확정하고 Phase 1~10의 개발·검증 경로를 만든다. 구현 Repository가 서로 다른 기준으로 출발하지 않게 하는 Architecture Baseline이다.

## 범위와 경계

- 포함: 전체 MSA 구조, Loan·Governance·Evaluation Run 상태 분리, 핵심 Envelope, Event 경계, Risk·Assurance 모델, Canonical Scenario, ADR, 기술·보안 기준, 전체 Roadmap과 추적 체계
- 제외: 실제 서비스 기능, 운영 배포, 실제 금융기관 연동
- 변경 금지: 실행 가능한 Schema는 `rippleguard-contracts`, 배포 설정은 `rippleguard-infra`, 서비스 내부 구현은 각 Repository가 소유한다.

## 참여 Repository

| Repository | 역할 |
| --- | --- |
| `rippleguard-docs` | 전체 Architecture·Roadmap·Baseline |
| `rippleguard-contracts` | 초기 API·Event·Agent·Policy 계약 |
| `rippleguard-infra` | 로컬 플랫폼 기본 구성과 통합 기준 |

## 선행 조건과 산출물

선행 Phase는 없다. 산출물은 프로젝트·MVP 기준, Canonical Scenario, 서비스 경계, 기술 스택, 상태 머신과 Evaluation Run, Decision·Consequence·Assurance 문서, Event 공통 기준, ADR, Phase 1~10 계획과 로컬 인프라 기본 기준이다.

## 통합 지점과 실패 경로

Docs의 설명용 모델을 Contracts의 실행 Schema와 연결하고, Infra가 고정 Contract·Image를 조합한다. 계약이나 소유권이 미정이면 Phase 1을 `READY`로 올리지 않는다.

## 완료 기준

- 개발자가 서비스 책임과 상태 소유권을 임의 해석하지 않아도 됨
- 계약 원본과 초기 Phase 1 입력 계약 확정
- 전체 Phase 1~10 목적·경계·인계 조건 문서화
- 문서 링크·의미 검증과 Phase 0 Baseline 고정

## 다음 Phase 인계 조건

Phase 1의 신청·상태 Event, 서비스별 DB 소유권, 로컬 Kafka·PostgreSQL 통합 기준이 구현 가능한 수준으로 준비되어야 한다.

Contracts와 Infra의 main 병합 결과는 [Cross-Repo Baseline](cross-repo-baseline.md)에 기록했다. Infra CI 상태 확인이 남아 있으므로 Phase 0은 아직 `VERIFIED`가 아니며 Phase 1은 `READY`로 전환하지 않는다.

## 실행 추적

- [Plan](plan.md)
- [Progress](progress.md)
- [Cross-Repo Baseline](cross-repo-baseline.md)
- [Verification](verification.md)
- [Troubleshooting](troubleshooting/README.md)
- [Evidence](evidence/README.md)
