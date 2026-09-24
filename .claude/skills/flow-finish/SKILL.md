---
name: flow-finish
description: Step 5 of this repo's GitHub flow — rebase merge the reviewed PR into develop and clean up branches. Use after flow-review has passed.
---

# 5. 머지 · 정리

흐름: flow-start → flow-work → flow-ship → flow-review → **flow-finish**

## 시작 전 확인

현재 브랜치 `feature/N-<요약>`의 PR 번호 `P`를 찾고, PR 체크리스트의 "PR diff를 직접 리뷰했다"가 체크되어 있는지 확인한다. 안 되어 있으면 멈추고 `flow-review`로 돌려보낸다.

```bash
gh pr view --json number,body -q '.number, .body'
```

## 절차

1. 머지
   ```bash
   gh pr merge P --rebase
   ```
2. 정리
   ```bash
   git push origin --delete feature/N-<요약>
   git switch develop
   git pull --prune
   git cherry develop feature/N-<요약>
   git branch -D feature/N-<요약>
   gh issue view N --json state -q .state
   ```
   - `git cherry` 결과가 전부 `-`일 때만 바로 다음 줄의 `git branch -D`를 실행한다. `+`가 하나라도 있으면 지우지 말고 멈춘다. `git cherry`는 `+`가 있어도 종료 코드가 0이라 `&&`로 이으면 안 된다
   - 이슈가 `CLOSED`가 아니면 PR 본문에 `Closes #N`이 있는지 확인하고 `gh issue close N --comment "Merged via #P"`

## 끝나면

머지된 커밋 목록, 삭제한 브랜치, 이슈 상태를 보고한다.

## 멈추는 조건

- `git cherry`에 `+`가 나올 때

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| rebase merge 후 `git branch -d`가 `not fully merged`로 거부 | GitHub rebase merge가 커밋을 새로 만들어 해시가 바뀜 | `git cherry`로 확인 후 `-D` |
| `Closes #N`인데 이슈가 안 닫힘 | 키워드는 기본 브랜치 머지 때만 동작 | 기본 브랜치는 `develop`. 그래도 OPEN이면 수동 종료 |
