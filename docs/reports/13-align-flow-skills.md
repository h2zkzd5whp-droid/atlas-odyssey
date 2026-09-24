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

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| `flow-ship` 3번 `gh pr view --json number,url -q '.number, .url'`가 `no pull requests found for branch "feature/13-align-flow-skills"`로 exit 1 | PR이 아직 없을 때의 정상 동작 | 고친 스킬 문구(`no pull requests found`면 새로 연다)대로 `gh pr create` 진행. 출력 URL 끝 숫자로 PR 번호(#14) 확인 |
| `/code-review 14` 뒤에도 로컬 `pr14`가 생기지 않음 | `.git/FETCH_HEAD`가 리뷰 중(01:53:34)에 갱신됐지만 내용은 `feature/13-align-flow-skills`·`develop`·`main` 세 줄뿐. `git fetch origin`은 했지만 `pull/14/head:pr14`는 받지 않음. 로컬 HEAD가 PR head와 같음 | 리뷰 대상이 PR head(`21a8aaf`)와 같으므로 결과는 유효 |

## 리뷰 결과 (flow-review, WSL)

- `flow-ship` 4번(CI): TODO 문구대로 명령 없이 건너뜀
- `flow-review` 1번: PR head(`gh pr view 14 --json headRefOid`)와 로컬 HEAD 모두 `21a8aaf53f979ac45ca4518dd3eaa54456090b0f`
- `/code-review 14` 전후 `git branch --list 'pr*'`: 둘 다 없음
- `/code-review` 결과: 발견 사항 3건(텍스트로 옴. 이번에도 "ReportFindings 도구 없음" 문구 있음). 이 PR에서는 고치지 않고 사용자 판단에 맡김
  1. (medium) `flow-start`의 새 작업 트리 확인: 스킬들이 `issue.md`·`msg.txt`·`pr.md`를 레포 루트에 쓰고 지우지 않으며 `.gitignore`에도 없다. 특히 `flow-review` "끝나면"의 `gh pr view P --json body -q .body > pr.md`가 `pr.md`를 남기면 다음 `flow-start`가 `?? pr.md`로 매번 멈춘다
  2. (low) `flow-ship` 3번 "번호가 나오면 그 PR을 쓴다": 인자 없는 `gh pr view`는 열린 PR이 없으면 닫히거나 머지된 PR도 돌려준다. 닫힌 PR이 있는 브랜치면 새 PR을 안 열고 닫힌 PR을 재사용하게 된다. `state`까지 받아 `OPEN`일 때만 재사용하자는 제안
  3. (low) `flow-review` 3번·`flow-finish` 2번의 `prP` 삭제 제거: #11·#13 두 번 모두 `prP`가 안 생겼지만, 로컬 HEAD가 PR head와 다를 때 생기는지는 미확인. 생긴다면 force push 뒤 재리뷰 때 옛 `prP`가 남아 옛 커밋을 볼 수 있다. 해가 없는 `git branch -D prP 2>/dev/null`을 남기자는 제안
- `gh pr diff 14`를 직접 읽음: 의도한 변경만 있음. 코드 펜스·목록 들여쓰기 깨짐 없음

## 남은 과제

- `docs/commands.md`에 `gh pr checks`와 `prP` 설명이 남아 있다. 아카이빙 예정이라 손대지 않았다
- 로컬 HEAD가 PR head와 다를 때 `/code-review`가 `prP`를 만드는지는 확인하지 않았다. 만든다면 `flow-start`의 작업 트리 확인과 별개로 남을 수 있다
- CI 워크플로를 추가할 때 `flow-ship` 4번을 작성한다
