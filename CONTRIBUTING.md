# Contributing Guide

devRavit 계정의 모든 repo 에 적용되는 공통 기여 규칙입니다. 각 repo 가 자기 `CONTRIBUTING.md` 를 가지지 않을 경우 이 문서가 자동 상속됩니다.

## 커밋 메시지

- **언어**: 한글
- **형식**: `{type}: {한글 메시지}`
- **예시**: `chore: Claude Code 불필요한 파일 탐색 방지를 위한 .claudeignore 추가`

### Type

| type | 사용 시점 |
|------|----------|
| `feat` | 새 기능 추가 |
| `fix` | 버그 수정 |
| `refactor` | 동작 변경 없는 구조 개선 |
| `perf` | 성능 개선 |
| `chore` | 빌드/설정/도구 변경 (코드 미변경) |
| `docs` | 문서만 변경 |
| `test` | 테스트만 추가·수정 |
| `style` | 포맷·세미콜론 등 기능 무관 변경 |

### 본문 (선택)

- 헤더 한 줄로 충분하지 않으면 빈 줄 후 본문에 **왜**(why) 위주로 기술.
- "무엇을"(what) 변경했는지는 diff 가 말해주므로 본문에 반복하지 않음.

## 브랜치 / PR

- **main 직접 푸시 금지**. 모든 변경은 feature branch → PR.
- 브랜치 이름: `{type}/{slug}` (예: `feat/lightsail-host-port-mapping`, `docs/readme-and-changelog-sync`).
- PR 본문은 자동 로드되는 PR 템플릿을 채워서 작성. 빈 섹션은 비워두기보다 "해당 없음" 명시.

## PR 라벨

- repo 별로 라벨 체계는 다를 수 있으나 공통 권장 type 라벨:
  - `documentation` — 문서·README·CHANGELOG
  - `ci` — GitHub Actions / CI / CD 워크플로
  - `bug` / `enhancement` — 기본 GitHub 라벨
- repo 별 도메인 라벨(예: deployment-hub 의 `App Runner` / `Lightsail`)은 그대로 유지.

## 보안

- 실제 ID/role/secret 값은 README·CHANGELOG·issue·PR 본문에 노출하지 않음 (placeholder 사용).
- 운영 설정 파일(`projects/*.yml`, `application-*.yml` 등)에 들어가는 실제값은 그대로 두되, 문서/예시에서는 마스킹.
