# Security Policy

## 취약점 보고

devRavit 계정의 어떤 repo 에서든 보안 취약점이나 민감 정보 노출을 발견하셨다면 **public 이슈로 올리지 마시고** 아래 채널로 보고해주세요.

- **Email**: devravit@gmail.com
- 제목 prefix: `[SECURITY] {repo} - {short summary}`

## 보고에 포함하면 좋은 정보

- 영향받는 repo / 브랜치 / commit SHA
- 재현 방법
- 영향 범위 추정 (정보 노출 / 권한 상승 / DoS 등)
- 가능하면 PoC

## 응대 절차

1. 24 시간 내 수신 확인 회신.
2. 검토 후 영향도 분류 (Critical / High / Medium / Low).
3. 패치 작성 — Critical/High 는 비공개 브랜치에서 작업 후 동시 공개.
4. 패치 머지 후 보고자 credit (원하실 경우) 명시.

## 지원 범위

| 항목 | 지원 여부 |
|------|----------|
| 운영 중인 repo 의 main 브랜치 | ✅ |
| archived repo | ❌ (수정 안 함, 노출 정보만 정리) |
| 의존성(외부 라이브러리) 취약점 | 우회 가능하면 업데이트, 아니면 issue 로 추적 |
