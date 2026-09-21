# 개발 방식

## TDD

모든 작업은 실패하는 테스트 작성 → 구현 → 리팩토링 순서로 진행한다.

## GitHub 흐름

Claude Code 스킬로 만들어서 사용한다. (스킬 미작성)

브랜치 전략·머지 방식·릴리스·커밋 컨벤션은 [CONTRIBUTING.md](../CONTRIBUTING.md), 단계별 명령어와 트러블슈팅은 [commands.md](commands.md) 참고.

1. 이슈 생성
2. 브랜치 생성 — `develop`에서 `feature/<이슈번호>-<요약>`
3. 작업 (TDD + 문서 갱신 — [문서 갱신 규칙](#문서-갱신-규칙))
4. 보고서 생성
5. 커밋
6. 푸시
7. PR 생성 — 본문에 `Closes #N`으로 이슈 연결 ([PR 템플릿](../.github/pull_request_template.md))
8. CI — 실패하면 3번으로 돌아간다
9. PR diff 리뷰
10. `develop`에 rebase merge — CI 통과 + 리뷰 완료 후에만
11. 정리 — 브랜치 삭제, 로컬 `develop` 갱신 + prune

## 문서 갱신 규칙

문서 갱신은 작업의 일부다. 코드만 바꾸고 문서를 안 바꾼 작업은 끝난 작업이 아니다. 같은 브랜치·같은 PR 안에서 코드와 함께 갱신한다.

### 도구를 추가할 때 (예: Python 포매터 black)

설치만 하고 끝내지 않는다. 아래 중 해당하는 것을 전부 같이 갱신한다.

- 의존성 목록 (`requirements.txt`, `pyproject.toml` 등)
- `.vscode/settings.json` — 기본 포매터, 저장 시 포맷 등
- `.vscode/extensions.json` — 필요한 VS Code 확장 추천
- `.editorconfig` — 들여쓰기·줄 길이가 도구 설정과 어긋나지 않게
- `.gitignore` — 도구가 만드는 캐시·산출물
- CI 워크플로 — 로컬과 같은 검사를 CI에서도 돌리도록
- 문서 — 설치·실행 방법, 이 파일

## 미정

- 보고서 위치: PR 본문 / 레포 내 파일
- CI: GitHub Actions 워크플로 (기술 스택 결정 후 작성)

## 배포 (잠정)

- 서버: Oracle Cloud Always Free (ARM A1, 2 OCPU / 12GB). 리전은 서울, 재고 없으면 춘천
- 예비: Google Cloud e2-micro (Oracle 가입 거절 시)
- 트리거: `main`에 머지되면 GitHub Actions로 배포. `main`에는 release만 들어가므로 발표 버전만 서버에 올라간다
- 서버 설정은 레포에 코드로 남긴다. 서버가 사라져도 다시 올릴 수 있어야 한다
- 스택 결정 후 확정: 1GB 환경(예비)에서도 돌아가는지, 구성(Docker, 리버스 프록시, HTTPS, 도메인)

## MCP

필요한 게 생기면 그때 연결.
