---
name: github-flow
description: Run this repo's issue → branch → commit → PR → review → rebase merge → cleanup flow for one task. Use when the user asks to start, ship, or finish a task, or to "run the flow".
---

# GitHub 흐름

한 작업을 [docs/dev-workflow.md](../../../docs/dev-workflow.md)의 11단계로 처리한다.

**시작 전에 반드시 읽는다:**
- `docs/commands.md` — 단계별 명령어와 트러블슈팅. 명령어는 여기 적힌 것을 쓴다
- `CONTRIBUTING.md` — 브랜치 이름, 커밋 컨벤션, 머지 방식

## 착수 판정

PR 제목을 `<type>: <summary>` 한 줄로 쓸 수 없거나 summary에 `and`가 들어가면 작업이 둘 이상이다. 사용자에게 나눠서 제안하고 첫 번째만 진행한다.

## 단계

1. **이슈** — 본문은 파일로 쓰고 `--body-file`로 넘긴다 (백틱 문제)
2. **브랜치** — develop을 pull --prune 한 뒤 `feature/<이슈번호>-<요약>`
3. **작업** — TDD 순서(실패하는 테스트 → 구현 → 리팩토링). 문서 갱신 규칙에 해당하는 문서·설정을 같은 브랜치에서 갱신
4. **보고서** — 위치 미정. 건너뛰고 사용자에게 알린다
5. **커밋** — 관심사별로 나눈다. 모든 커밋이 develop에 그대로 올라가므로(rebase merge) 각각 컨벤션을 지킨다
6. **푸시**
7. **PR** — `--base develop`, 본문은 `.github/pull_request_template.md` 형식을 채워 `--body-file`로. 첫 줄 `Closes #<이슈번호>`
8. **CI** — 워크플로가 없으면 건너뛴다. 실패하면 3단계로 돌아간다
9. **리뷰** — 마지막 푸시 후에 `/code-review <PR번호>`를 돌린다. 결과가 비어 와도 "문제 없음"으로 보지 않는다 (low 리뷰는 문서 렌더링 문제를 놓친 적이 있다). 실제 관문은 직접 리뷰다: PR head가 로컬 HEAD와 같은지 확인한 뒤 `gh pr diff <PR번호>` 전체를 읽고, 문서는 렌더링(링크 앵커, 코드 스팬)까지 확인한다. 발견 사항이 있으면 3단계로 돌아가 고친다. 끝나면 PR 체크리스트를 갱신한다
10. **머지** — `gh pr merge <PR번호> --rebase`
11. **정리** — commands.md 11단계 그대로. `/code-review`가 남긴 `pr<PR번호>` 로컬 브랜치도 같이 지운다. `git cherry` 결과에 `+`가 있으면 지우지 말고 멈춘다. 이슈가 닫혔는지 확인한다

## 멈추는 조건

- develop·main에 직접 커밋·푸시가 필요해 보일 때 (룰셋이 막는다. 우회하지 않는다)
- `git cherry`에 `+`가 나올 때
- 리뷰 발견 사항을 고칠지 판단이 필요할 때

## 끝나면

각 단계 결과(이슈·PR 링크, 커밋 목록, 건너뛴 단계와 이유)를 사용자에게 보고한다. 흐름 중 새로운 문제가 생겼으면 `docs/commands.md` 트러블슈팅에 추가한다.

## 실행 환경

Windows 쪽에서 `\\wsl.localhost\...` 경로로 작업 중이면 git·gh는 WSL 안에서 돌린다. 명령을 스크립트 파일로 쓰고 `MSYS_NO_PATHCONV=1 wsl.exe -d Ubuntu -- bash /mnt/c/.../script.sh`로 실행한다 (commands.md 트러블슈팅 참고). WSL·macOS 터미널에서 직접 작업 중이면 그대로 실행한다.
