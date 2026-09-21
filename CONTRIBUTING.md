# Contributing

작업 흐름 전체는 [docs/dev-workflow.md](docs/dev-workflow.md) 참고.

## 브랜치 전략 — Git Flow 간소화

| 브랜치 | 수명 | 역할 | 따는 곳 | 머지 대상 |
| --- | --- | --- | --- | --- |
| `main` | 상시 | 발표 버전만. 릴리스마다 태그 | — | — |
| `develop` | 상시 | 평소 작업이 모이는 곳 | — | — |
| `feature/<이슈번호>-<요약>` | 작업 하나 | 개별 작업 | `develop` | `develop` |
| `release/<버전>` | 발표 준비 기간 | 발표 버전 동결 | `develop` | `main`, `develop` |

- `main`, `develop`에 직접 커밋하지 않는다. 모든 변경은 PR로 들어간다.
- 작업 하나당 feature 브랜치 하나. 예: `feature/12-login-form`
- `hotfix`는 쓰지 않는다 (배포 환경 없음).

## 머지 방식

| PR | 방식 | 이유 |
| --- | --- | --- |
| `feature/*` → `develop` | rebase merge | TDD 커밋(test → feat → refactor)을 한 줄 히스토리에 그대로 남긴다 |
| `release/*` → `main` | merge commit | 릴리스 시점을 히스토리에 표시한다 |
| `release/*` → `develop` | merge commit | release에서 고친 내용을 develop에 되돌린다 |

rebase merge는 커밋이 전부 그대로 올라가므로 커밋 하나하나가 아래 컨벤션을 지켜야 한다. 머지 후 커밋 해시가 바뀌므로 feature 브랜치는 머지 직후 삭제한다.

## 릴리스

| 버전 | 시점 |
| --- | --- |
| `v0.1.0` | 중간 발표 |
| `v1.0.0` | 최종 발표 |

1. 발표 1~2주 전 `develop`에서 `release/<버전>`을 딴다.
2. release에서는 버그 수정, 문서 정리, 버전 번호 변경만 한다. 새 기능은 develop에서 계속한다.
3. release → `main` 머지 후 `v<버전>` 태그를 단다. `main` 머지는 곧 배포다 ([배포](docs/dev-workflow.md#배포-잠정)).
4. release → `develop` 머지.
5. release 브랜치를 삭제한다.

## 커밋 컨벤션 — Conventional Commits

```
<type>(<scope>): <summary>
```

- `scope`는 생략 가능.
- `summary`는 영어, 명령형, 소문자로 시작, 마침표 없음.

| type | 용도 |
| --- | --- |
| `feat` | 기능 추가 |
| `fix` | 버그 수정 |
| `test` | 테스트 추가·수정 |
| `refactor` | 동작 변경 없는 코드 개선 |
| `docs` | 문서 |
| `style` | 포맷팅 (동작 변경 없음) |
| `chore` | 설정, 의존성 등 기타 |
| `ci` | CI 설정 |

TDD 작업의 커밋 예:

```
test: add failing cases for date parsing
feat: implement date parsing
refactor: extract parse helper
```
