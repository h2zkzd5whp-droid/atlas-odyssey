---
name: flow-start
description: Step 1 of this repo's GitHub flow — create an issue and a feature branch for one task. Use when the user asks to start a new task.
---

# 1. 이슈 · 브랜치

흐름: **flow-start** → flow-work → flow-ship → flow-review → flow-finish

브랜치 이름·커밋 컨벤션은 `CONTRIBUTING.md`를 따른다.

## 착수 판정

PR 제목을 `<type>: <summary>` 한 줄로 쓸 수 없거나 summary에 `and`가 들어가면 작업이 둘 이상이다. 사용자에게 나눠서 제안하고 첫 번째만 진행한다.

## 절차

1. 이슈 생성. 본문은 파일로 쓰고 `--body-file`로 넘긴다
   ```bash
   gh issue create --title "..." --body-file issue.md
   ```
2. 브랜치 생성. `N`은 이슈 번호, `<요약>`은 짧은 영어 요약(예: `login-form`)
   ```bash
   git switch develop
   git pull --prune
   git switch -c feature/N-<요약>
   ```

## 끝나면

이슈 링크와 브랜치 이름을 보고하고 멈춘다. 다음은 `flow-work`.

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| `--body "..."` 안의 백틱 부분이 `command not found`를 내고 사라짐 | 큰따옴표 안 백틱은 bash·zsh 명령 치환 | `--body-file` 사용 |
