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

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| `flow-review`를 Skill 도구로 불렀는데 3번이 고치기 전 문구로 로드됨 | 스킬 내용이 세션 시작 때 읽힌 것으로 보임. 작업 트리의 수정본이 같은 세션에서는 반영되지 않음 | 새 3번 지시를 직접 적용. 스킬을 고친 세션에서 그 스킬을 검증하려면 새 세션이 필요함 |
| `/code-review 10` 뒤에 로컬 `pr10`이 생기지 않음 | Windows 쪽에서 돌아간 `/code-review`가 로컬 git의 `dubious ownership`에 막혀 `git fetch` 대신 `gh`로 원격 저장소를 지정해 diff를 읽음 | 리뷰는 원격 PR diff(최신 head)를 봤으므로 결과는 유효. `prP`가 생기는 경로는 이번에 확인하지 못함. 전역 git 설정(`safe.directory`)은 바꾸지 않음 |

## 리뷰 반영 (1차)

직접 리뷰에서 발견한 1건을 고쳤다.

- 이 보고서의 트러블슈팅이 "없음"이었는데 실제로 겪은 문제 2건(스킬 로드 캐시, `pr10` 미생성)을 적지 않았다. 표로 채웠다.

## 남은 과제

- `docs/commands.md` 156행도 `prP`를 정리 단계에서 지운다고만 적혀 있다. 아카이빙 예정 문서라 이번에는 고치지 않았다.
- WSL·macOS 터미널에서 `/code-review`가 `prP`를 만드는 경우에 새 3번 지시가 재리뷰의 옛 커밋 문제를 막는지 확인
