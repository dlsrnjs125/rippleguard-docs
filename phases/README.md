# Phase Documentation

각 Phase 폴더는 목표와 범위, 실제 진행 상태, 함께 검증된 멀티레포 Baseline, 검증 결과, 트러블슈팅과 증빙을 한곳에 연결한다.

## 새 Phase 시작 방법

1. [Phase README Template](../templates/phase-readme-template.md)을 복사해 `phase-NN-short-name/README.md`를 만든다.
2. 해당 Phase에 필요한 `plan.md`, `progress.md`, `cross-repo-baseline.md`, `verification.md`만 생성한다.
3. 관련 Repository와 선행 계약, 통합 지점을 `TBD` 추측 없이 기록한다.
4. [전체 Phase Status](../tracking/phase-status.md)를 `IN_PROGRESS`로 갱신한다.

Phase 종료에는 범위 내 산출물, 계약 호환성, 통합 검증, 잔여 위험이 검토되어야 한다. Baseline에는 Branch뿐 아니라 검증된 Commit SHA, 불변 Image, 계약·Migration 버전과 Verification을 기록한다. 문제는 가치가 있을 때만 `troubleshooting/`에 남기고 전체 Index에 연결한다. Evidence에는 실제 생성된 검사 결과와 재현 가능한 참조만 보관한다.
