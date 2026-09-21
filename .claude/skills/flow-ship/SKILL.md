---
name: flow-ship
description: Step 3 of this repo's GitHub flow — commit by concern, push, open or update the PR into develop, and check CI. Use after flow-work.
---

# 3. 커밋 · 푸시 · PR · CI

흐름: flow-start → flow-work → **flow-ship** → flow-review → flow-finish

커밋 컨벤션은 `CONTRIBUTING.md`를 따른다.

## 시작 전 확인

현재 브랜치가 `feature/N-<요약>`인지, `docs/reports/N-<요약>.md`가 있는지 확인한다. 없으면 멈추고 `flow-work`로 돌려보낸다.

## 절차

1. 커밋. 관심사별로 나눈다. rebase merge라 모든 커밋이 develop에 그대로 올라가므로 커밋마다 컨벤션을 지킨다. 보고서는 `docs: add report for #N` 커밋으로
   ```bash
   git add <파일>
   git commit -m "<type>: <summary>"
   ```
   메시지에 백틱이 필요하면 `git commit -F msg.txt`
2. 푸시
   ```bash
   git push -u origin feature/N-<요약>
   ```
3. PR. 이미 열려 있으면(`gh pr view --json number`) 생성은 건너뛴다. 본문은 `.github/pull_request_template.md` 형식을 채우고 첫 줄 `Closes #N`, 작업 내용에 보고서 경로를 적는다. "PR diff를 직접 리뷰했다"는 체크하지 않는다
   ```bash
   gh pr create --base develop --title "<type>: <summary>" --body-file pr.md
   ```
   새로 열었든 이미 있었든 PR 번호 `P`를 확인한다
   ```bash
   gh pr view --json number,url -q '.number, .url'
   ```
4. CI. 워크플로가 없으면 건너뛴다
   ```bash
   gh pr checks P --watch
   ```

## 끝나면

- CI 통과(또는 없음): PR 링크와 커밋 목록을 보고하고 멈춘다. 다음은 `flow-review`
- CI 실패: 실패 내용을 보고하고 멈춘다. 다음은 `flow-work`

## 멈추는 조건

- develop·main에 직접 커밋·푸시가 필요해 보일 때 (룰셋이 막는다. 우회하지 않는다)

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| `--body "..."` 안의 백틱 부분이 `command not found`를 내고 사라짐 | 큰따옴표 안 백틱은 bash·zsh 명령 치환 | `--body-file`, 커밋은 `-F` |
