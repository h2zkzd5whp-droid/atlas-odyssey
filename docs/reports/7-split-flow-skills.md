# Split github-flow skill into per-step flow skills (#7)

## 작업 내용

- 모든 단계를 담고 있던 `github-flow` 스킬을 단계별 스킬 5개로 나눴다: `flow-start`(이슈·브랜치) → `flow-work`(작업·보고서) → `flow-ship`(커밋·푸시·PR·CI) → `flow-review`(리뷰) → `flow-finish`(머지·정리). 각 스킬은 자기 단계만 하고 멈춘다.
- 스킬마다 반복되던 공통 규칙·환경(작업 흐름, 문서 갱신 규칙, GitHub 룰셋, Windows 쪽 실행 방법, 셸 주의점)을 루트 `CLAUDE.md`로 옮겼다. 스킬이 `CLAUDE.md`에 기대므로 같은 PR로 넣는다.
- `CONTRIBUTING.md` 첫 줄 링크를 `docs/dev-workflow.md` 대신 새 스킬 5개로 바꾸고, 릴리스 절차의 `docs/dev-workflow.md#배포-잠정` 링크를 뺐다.
- 문서 갱신 규칙에 따라 `docs/dev-workflow.md`에 남아 있던 옛 `github-flow` 스킬 링크를 새 스킬 5개로 바꿨다. 이 문서는 아카이빙 예정이라 링크 한 줄만 고쳤다.

## 변경 파일

- `.claude/skills/flow-start/SKILL.md` (신규)
- `.claude/skills/flow-work/SKILL.md` (신규)
- `.claude/skills/flow-ship/SKILL.md` (신규)
- `.claude/skills/flow-review/SKILL.md` (신규)
- `.claude/skills/flow-finish/SKILL.md` (신규)
- `.claude/skills/github-flow/SKILL.md` (삭제)
- `CLAUDE.md` (신규)
- `CONTRIBUTING.md`
- `docs/dev-workflow.md`
- `docs/reports/7-split-flow-skills.md` (이 보고서)

## 테스트

코드 변경이 없어 자동 테스트는 없다. 대신 이 작업 자체를 새 스킬 5개로 처음부터 끝까지(이슈 #7 생성부터 머지·정리까지) 실행해 검증한다.

- `grep -rn "github-flow"`로 옛 스킬을 가리키는 링크가 남지 않았는지 확인
- 새 스킬 링크 경로가 실제 파일과 맞는지 확인

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| Windows 쪽에서 실행한 `wsl.exe ... bash script.sh 2>&1` 출력에서 stdout과 stderr 줄이 뒤섞여 잘림(예: `Your branch is up to date with 'ori+ git branch -a`) | `wsl.exe`가 두 스트림을 따로 넘겨 Git Bash가 합칠 때 순서가 섞임. `set -x` 추적 출력이 stderr라 더 심해짐 | `set -x`를 빼고, 순서가 중요하면 스크립트 안에서 `exec 2>&1`로 합친다 |
| `flow-start`의 `git switch develop` → `git pull --prune` → `git switch -c`를 커밋 안 된 변경이 있는 상태에서 실행 | 이번 작업은 develop 작업 트리의 미커밋 변경을 그대로 옮겨 싣는 경우. 스킬에 이 경우 설명이 없음 | 원격에 새 커밋이 없어 `Already up to date`였고, 미커밋 변경은 새 브랜치로 그대로 따라왔다 |
| 옛 스킬 삭제 후 `docs/dev-workflow.md`의 `github-flow` 링크가 끊김 | `CONTRIBUTING.md`만 갱신 대상에 있었음 | 링크 한 줄을 새 스킬로 교체 |

## 리뷰 반영 (1차)

`/code-review 8` 발견 사항 2건을 고쳤다.

- `flow-ship`: 4단계 `gh pr checks P --watch`의 `P`를 얻는 방법이 없었다. PR이 이미 열려 있으면 3단계 전체를 건너뛰게 되어 있어서 더 그랬다. 3단계를 "생성은 건너뛴다"로 바꾸고, 새로 열었든 이미 있었든 `gh pr view --json number,url`로 `P`를 확인하게 했다.
- `flow-review`: 체크리스트 체크를 `gh pr edit P --body-file pr.md`로 하는데 `pr.md`는 `flow-ship`이 남긴 로컬 파일이다. 없으면 실패하고, 오래됐으면 GitHub에서 고친 본문을 덮어쓴다. 현재 본문을 `gh pr view P --json body -q .body > pr.md`로 받아서 고친 뒤 올리게 했다.

## 남은 과제

- `docs/commands.md`, `docs/dev-workflow.md` 아카이빙 (`CLAUDE.md`에 예정으로 적혀 있음)
