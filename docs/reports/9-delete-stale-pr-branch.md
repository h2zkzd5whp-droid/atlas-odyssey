# chore: delete stale pr branch before code review in flow-review (#9)

## 작업 내용

- `/code-review P`는 `git fetch origin pull/P/head:prP`로 로컬 브랜치 `prP`를 남긴다. 수정 후 재리뷰할 때 이 브랜치가 첫 리뷰 때 커밋을 그대로 가리키고 있어서, 이전 테스트에서 두 번째 리뷰가 옛 코드를 봤을 가능성이 있었다.
- `flow-review` `## 절차` 3번에, `/code-review`를 돌리기 전에 이전 리뷰가 남긴 `prP`가 있으면 `git branch -D prP`로 지우라는 지시와 이유(옛 커밋을 볼 수 있음, PR head는 원격 feature 브랜치에 있어 지워도 안전함)를 넣었다. 다른 단계는 건드리지 않았다.

## 변경 파일

- `.claude/skills/flow-review/SKILL.md`
- `docs/reports/9-delete-stale-pr-branch.md` (이 보고서)

## 테스트

코드 변경이 없어 자동 테스트는 없다. 이 작업 자체를 스킬 5개로 처음부터 끝까지 실행하면서, `flow-review`에서 새 3번 지시대로 `prP`를 지우고 `/code-review`를 돌린 뒤 로컬 `prP`가 PR head와 같은 커밋을 가리키는지 확인한다.

## 트러블슈팅

없음

## 남은 과제

- `docs/commands.md` 156행도 `prP`를 정리 단계에서 지운다고만 적혀 있다. 아카이빙 예정 문서라 이번에는 고치지 않았다.
