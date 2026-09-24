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

없음

## 남은 과제

- flow-review 단계에서 `/code-review` 실행 전후의 `prP` 상태와 리뷰 결과를 기록한다.
