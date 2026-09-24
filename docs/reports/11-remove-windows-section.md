# docs: remove windows-side workaround section from claude.md (#11)

## 작업 내용

- 이제 작업은 WSL 안(VS Code Remote-WSL 포함)에서 직접 한다. 루트 `CLAUDE.md`의 `### Windows 쪽에서 실행할 때` 섹션(제목, 문단, 불릿 3개, `bash` 코드 블록)은 Windows 쪽 PowerShell·Git Bash에서 `\\wsl.localhost\...` 경로로 작업할 때의 우회법이라 더 이상 해당하지 않아 지웠다.
- 위의 `## 환경` 불릿과 아래 `### 셸` 섹션은 그대로 뒀다.
- 같은 내용이 `docs/commands.md` 166~172행에도 있지만 아카이빙 예정 문서라 건드리지 않았다. `docs/reports/7-*`, `9-*`의 Windows 관련 기록은 당시 기록이라 그대로 뒀다.

## 변경 파일

- `CLAUDE.md`
- `docs/reports/11-remove-windows-section.md` (이 보고서)

## 테스트

코드 변경이 없어 자동 테스트는 없다. `git diff`로 `CLAUDE.md`에서 섹션 12행(뒤 빈 줄 포함)만 빠지고 `## 환경`과 `### 셸`이 바로 이어지는지 확인했다. 이 작업 자체를 WSL 안에서 스킬 5개(flow-finish 제외)로 실행해, #9 보고서의 남은 과제(WSL에서 `/code-review`가 `prP`를 만드는지)를 확인한다.

## 트러블슈팅

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| `flow-ship` 3번 `gh pr view --json number`가 `no pull requests found for branch ...`로 exit 1 | PR이 아직 없을 때의 정상 동작 | 무시하고 `gh pr create`로 진행. `&&`로 묶으면 뒤 명령이 안 돌므로 따로 실행 |
| `flow-ship` 4번 `gh pr checks 12`가 `no checks reported on the 'feature/11-remove-windows-section' branch`로 exit 1 | `.github/workflows`가 없음 (CI 없음) | 스킬 지시대로 건너뜀 |
| `flow-review` 3번 `git branch -D pr12`가 `error: branch 'pr12' not found`로 exit 1 | 이전 리뷰가 없어서 `pr12`가 없음 | 스킬에 적힌 대로 무시 |
| `/code-review 12` 뒤에도 로컬 `pr12`가 생기지 않음 | `/code-review`가 `git fetch origin pull/12/head:pr12`를 하지 않음. `.git/FETCH_HEAD`가 flow-start의 `git pull --prune` 시각(01:14:13) 그대로이고 내용도 `develop`·`main` 두 줄뿐이라 리뷰 중 fetch 자체가 없었음. 로컬 HEAD가 PR head와 같아서 작업 트리·`gh`로 읽은 것으로 보임 | 리뷰 대상은 PR head(`8d3bd97`)와 같으므로 결과는 유효. #9의 "WSL에서는 `prP`가 생긴다"는 가정은 이번에 확인되지 않음 |

## 리뷰 결과 (flow-review, WSL)

- `/code-review 12` 실행 전 `git branch --list 'pr*'`: 없음
- 실행 후 `git branch --list 'pr*'`: 없음. `pr12`가 만들어지지 않아 tip과 PR head(`gh pr view 12 --json headRefOid` = `8d3bd9734e604e9ed40a9fe5e37a9727a73c5d69`)는 비교할 대상이 없음. 로컬 HEAD는 PR head와 같음
- `/code-review` 결과: 발견 사항 0건. 빈 `(none)`이 아니라 확인 내용이 텍스트로 옴(섹션 12행만 빠짐, 코드 펜스 깨짐 없음, 남은 참조는 `docs/commands.md`·보고서 7·9뿐). 다만 결과에 "ReportFindings 도구가 세션에 없어 호출 못 함"이라는 문구가 있었음
- git 소유권·권한 에러(`dubious ownership` 등): 없음. `safe.directory` 설정 없이 모든 git 명령이 통과함
- `gh pr diff 12`를 직접 읽음: 의도한 변경만 있음

## 남은 과제

- `flow-review` 3번의 `prP` 삭제 지시는 `/code-review`가 `prP`를 만든다는 전제인데, WSL에서도 만들지 않았다. 로컬 HEAD가 PR head와 다를 때(예: 다른 브랜치에서 리뷰할 때) `prP`가 생기는지는 아직 확인 못 함. 3번 문구를 유지할지 별도 이슈로 판단
- `/code-review`가 "ReportFindings 도구 없음"을 알린 원인 미확인 (서브에이전트 세션이라서일 수 있음)
