---
name: flow-review
description: Step 4 of this repo's GitHub flow — the user reviews the PR diff and Claude runs /code-review. Use after flow-ship.
---

# 4. 리뷰

흐름: flow-start → flow-work → flow-ship → **flow-review** → flow-finish

리뷰는 두 개다. **사용자의 diff 리뷰**와 **Claude의 `/code-review`**. 둘 다 통과해야 다음으로 간다.

## 시작 전 확인

현재 브랜치 `feature/N-<요약>`의 PR 번호 `P`를 찾는다. PR이 없으면 멈추고 `flow-ship`으로 돌려보낸다.

```bash
gh pr view --json number,url -q '.number, .url'
```

## 절차

1. PR head가 로컬 HEAD와 같은지 확인한다. 다르면 몇 초 뒤 다시 확인한다 (푸시 직후 지연)
   ```bash
   gh pr view P --json headRefOid -q .headRefOid
   git rev-parse HEAD
   ```
2. 사용자에게 PR 링크를 주고 diff 리뷰를 요청한다
3. `/code-review P`를 돌린다. 결과가 비어 와도 통과로 보지 않는다 (문서 버그를 놓친 적이 있다). `gh pr diff P`를 직접도 읽고, 문서는 렌더링(링크 앵커, 코드 스팬)까지 확인한다
4. Claude의 발견 사항을 보고하고 사용자의 리뷰 결과를 기다린다

## 끝나면

- 발견 사항이 있음(사용자 또는 Claude): 목록을 정리해 보고하고 멈춘다. 다음은 `flow-work`
- 사용자가 통과를 확인함: PR 체크리스트의 "PR diff를 직접 리뷰했다"를 체크하고(`gh pr edit P --body-file pr.md`) 멈춘다. 다음은 `flow-finish`

사용자의 통과 확인 없이 체크하거나 다음 단계로 넘어가지 않는다.

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| 푸시 직후 `gh pr diff`가 이전 내용 | GitHub의 PR 갱신 지연 | 1번: head 일치 확인 후 리뷰 |
| `/code-review` 결과가 `(none)` | 확인 못 함. 발견 사항이 텍스트로 온 적도 있음 | 직접 확인을 생략하지 않음 |
| `gh pr edit`가 `Projects (classic) is being deprecated`로 실패 | Ubuntu 기본 저장소의 구버전 gh(2.45) | GitHub 공식 apt 저장소에서 최신 gh 설치. macOS는 `brew install gh` |
