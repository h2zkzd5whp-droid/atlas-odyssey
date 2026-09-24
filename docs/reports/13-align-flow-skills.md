# chore: align flow skills with wsl test results (#13)

## 작업 내용

#11을 WSL에서 flow 스킬로 끝까지 돌려본 결과에 맞춰 스킬 설명을 고쳤다.

- `flow-start`: "시작 전 확인" 추가. `git status --short`에 출력이 있으면 멈춘다
- `flow-ship` 3번: 먼저 `gh pr view`로 이 브랜치의 PR을 확인하고, 번호가 나오면 그 PR을 쓰고, `no pull requests found`면(에러 아님) 새로 연다. PR 번호는 `gh pr create` 출력 URL에서 읽는다
- `flow-ship` 4번: `gh pr checks` 명령을 빼고 TODO로 바꿨다. CI 워크플로가 없으니 돌릴 게 없다. "끝나면"의 CI 실패 분기도 뺐다
- `flow-review` 3번, `flow-finish` 2번: `prP` 브랜치 삭제 지시를 뺐다. #11에서 `/code-review`가 `prP`를 만들지 않았다

## 변경 파일

- `.claude/skills/flow-start/SKILL.md`
- `.claude/skills/flow-ship/SKILL.md`
- `.claude/skills/flow-review/SKILL.md`
- `.claude/skills/flow-finish/SKILL.md`
- `docs/reports/13-align-flow-skills.md` (이 보고서)

## 테스트

코드 변경이 없어 자동 테스트는 없다. 스킬은 세션 시작 때 읽히므로, 고친 스킬로 새 세션에서 `flow-ship`·`flow-review`를 돌려 확인한다.

## 트러블슈팅

없음

## 남은 과제

- `docs/commands.md`에 `gh pr checks`와 `prP` 설명이 남아 있다. 아카이빙 예정이라 손대지 않았다
- 로컬 HEAD가 PR head와 다를 때 `/code-review`가 `prP`를 만드는지는 확인하지 않았다. 만든다면 `flow-start`의 작업 트리 확인과 별개로 남을 수 있다
- CI 워크플로를 추가할 때 `flow-ship` 4번을 작성한다
