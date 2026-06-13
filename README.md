# .github

devRavit 계정 전역 **community health files** 기본값 + 공용 reusable workflow 저장소.

## TL;DR

GitHub 은 user/org 계정에 `.github` 라는 이름의 public repo 가 존재하면, 그 안의 community health files (PR 템플릿, CONTRIBUTING, 이슈 템플릿 등) 를 **같은 계정의 다른 모든 repo 가 자기 파일을 가지지 않을 때** 기본값으로 사용합니다.

공식 문서: [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)

## 1. 자동 상속되는 파일 (Community Health Files)

각 repo 가 같은 이름 파일을 가지지 않을 때 이 repo 의 파일이 자동으로 표시됨.

| 파일 | 용도 | 상태 |
|------|------|-----|
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | PR 작성 시 기본 로드되는 한글 템플릿 (작업 배경 / 의사결정 / 패턴 / 방향 / 효과 / 개발 방법 / 테스트 / 위험 / 확장 / 문서) | ✅ 구현 |
| [`.github/ISSUE_TEMPLATE/bug_report.md`](.github/ISSUE_TEMPLATE/bug_report.md) | 버그 리포트 양식 | ✅ 구현 |
| [`.github/ISSUE_TEMPLATE/feature_request.md`](.github/ISSUE_TEMPLATE/feature_request.md) | 기능 제안 양식 | ✅ 구현 |
| [`.github/ISSUE_TEMPLATE/config.yml`](.github/ISSUE_TEMPLATE/config.yml) | 빈 이슈 차단 + 외부 채널 안내 | ✅ 구현 |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | 한글 커밋 컨벤션 / 브랜치 / PR / 라벨 / 보안 가이드 | ✅ 구현 |
| [`SECURITY.md`](SECURITY.md) | 보안 이슈 보고 채널 | ✅ 구현 |
| `CODE_OF_CONDUCT.md` | 행동 강령 | ⏳ 미구현 |
| `SUPPORT.md` | 사용자 지원 채널 안내 | ⏳ 미구현 |
| `FUNDING.yml` | GitHub Sponsors / Patreon 등 후원 링크 | ⏳ 해당 없음 |

## 2. Reusable Workflows (각 repo 가 명시적으로 호출)

워크플로 파일은 inherit 되지 않습니다. 단, 이 repo 에 `on: workflow_call` 형식으로 두면 다른 repo 가 한 줄로 호출 가능 — **로직 중앙화, 호출은 repo 별 opt-in**.

| 워크플로 | 용도 | 호출 방법 | 상태 |
|---------|------|----------|-----|
| [`.github/workflows/commitlint.yml`](.github/workflows/commitlint.yml) | PR 의 모든 커밋 메시지가 `{type}: {message}` 컨벤션 준수하는지 검증 | 아래 §4 예시 참조 | ✅ 구현 (experimental) |
| `pr-title.yml` | PR 타이틀 자체가 컨벤션 따르는지 검증 (squash merge 대비) | — | ⏳ 미구현 |
| `labeler.yml` | 변경 파일 경로 기반 자동 PR 라벨링 (`docs/**` → `documentation` 등) | — | ⏳ 미구현 |
| `release-please.yml` | conventional commits 기반 CHANGELOG / Release 자동화 | — | ⏳ 미구현 |

## 3. 자동 상속 불가 — 각 repo 자체 관리 필요

| 항목 | 이유 | 대안 |
|------|------|-----|
| `.gitignore` | GitHub inherit 메커니즘 없음 + 스택별로 상이 (Spring / Next.js / Lua / Python) | 각 repo 직접 관리. 공통 시작점은 [github.com/github/gitignore](https://github.com/github/gitignore) |
| `.gitmessage` | GitHub 영역이 아닌 **로컬 git config** | [`docs/gitmessage.txt`](docs/gitmessage.txt) 표준 템플릿 호스팅. 사용자가 로컬에서 `git config --global commit.template <path>` 셋업 |
| 라벨 정의 | 라벨은 repo 별 분리 | (미구현) label-sync 워크플로 후보 — §5 참조 |
| 일반 GitHub Actions 워크플로 | `.github/workflows/*.yml` inherit 안 됨 | reusable workflow 로 만들고 각 repo opt-in |
| Branch protection 규칙 | API/UI 설정, 파일 없음 | (미구현) 셋업 스크립트 후보 |

## 4. 사용 방법

### PR 템플릿 / CONTRIBUTING / ISSUE_TEMPLATE / SECURITY

별도 작업 불필요. 각 repo 에 동일 이름 파일이 없으면 자동 로드.

### `.gitmessage` 로컬 셋업

```sh
# 1) 이 repo 의 표준 템플릿을 로컬에 두기
curl -fsSL https://raw.githubusercontent.com/devRavit/.github/main/docs/gitmessage.txt \
  -o ~/.gitmessage

# 2) git 전역 설정에 등록
git config --global commit.template ~/.gitmessage
```

이후 `git commit` 만 입력하면 에디터에 템플릿이 자동 로드됨.

### Reusable workflow 호출 (commitlint 예시)

각 repo 에 다음 파일 추가:

```yaml
# .github/workflows/lint.yml
name: Lint

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  commitlint:
    uses: devRavit/.github/.github/workflows/commitlint.yml@main
```

> production 호출 시 `@main` 대신 태그 핀(`@v1`) 권장. 현재 tag 미발행 — 실서비스 도입 전 v1 릴리스 예정.

## 5. 추가 보완 후보 (로드맵)

순서는 우선순위 무관, 필요할 때 PR 로 추가.

- [ ] `CODE_OF_CONDUCT.md` 추가
- [ ] `SUPPORT.md` 추가 (사용자 지원 채널)
- [ ] `pr-title.yml` reusable workflow — squash merge 시 머지 커밋 메시지가 PR 타이틀이 되므로 타이틀 컨벤션 검증 필요
- [ ] `labeler.yml` reusable workflow + 표준 [`labeler config`](https://github.com/actions/labeler) — `docs/**`, `.github/workflows/**`, `src/**` 등 경로 기반 자동 라벨
- [ ] `release-please.yml` reusable workflow — conventional commit 누적 → CHANGELOG / GitHub Release 자동
- [ ] **label-sync 워크플로** — 이 repo 에 `labels.yml` 두고 다른 repo 에 라벨 정의(이름/색/설명) 동기화. [`micnncim/action-label-syncer`](https://github.com/micnncim/action-label-syncer) 등 활용
- [ ] **branch-protection 셋업 스크립트** — `main` 직접 푸시 차단 + CI 통과 필수 규칙을 모든 repo 에 일괄 적용
- [ ] **auto-merge 워크플로** — `documentation` 라벨 + 모든 체크 통과 시 자동 머지
- [ ] **stale 이슈/PR 자동 닫기** — 90 일 무응답 시 라벨링 후 닫기
- [ ] **이 repo 의 CHANGELOG 관리** — community files 자체 변경 이력 추적

## 6. 적용 범위 / 우선순위

- 이 repo 가 **public** 이라 같은 계정의 **public + private 전체** repo 가 상속받음. (private `.github` 였으면 private repo 만)
- 각 repo 가 자기 같은 이름 파일을 가지면 그 파일이 **우선**. 진정한 단일 소스 유지하려면 per-repo 파일을 두지 말 것.
- 의도적으로 다른 양식이 필요한 repo (예: 외부 contributor 가 많은 OSS) 만 자기 파일 유지.

## 7. 관련

- **CD 워크플로 중앙화**: [devRavit/deployment-hub](https://github.com/devRavit/deployment-hub) — App Runner / Lightsail / CurseForge 배포 워크플로 호스팅
- **프로필 페이지**: [devRavit/devRavit](https://github.com/devRavit/devRavit) — github.com/devRavit 상단 노출
- **공식 문서**: [Default community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
