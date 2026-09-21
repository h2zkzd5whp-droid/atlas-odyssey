---
name: flow-work
description: Step 2 of this repo's GitHub flow — implement the task with TDD on the current feature branch and write its report. Use after flow-start, or when CI or review sends the task back for fixes.
---

# 2. 작업 · 보고서

흐름: flow-start → **flow-work** → flow-ship → flow-review → flow-finish

## 시작 전 확인

현재 브랜치가 `feature/N-<요약>`인지 확인한다. 아니면 멈추고 사용자에게 알린다. `N`과 `<요약>`은 브랜치 이름에서 읽는다.

```bash
git branch --show-current
```

## 작업

- TDD 순서: 실패하는 테스트 → 구현 → 리팩토링
- CLAUDE.md의 문서 갱신 규칙대로 관련 문서·설정도 이 브랜치에서 갱신한다
- CI 실패나 리뷰 발견 사항으로 돌아온 경우: 지적된 것만 고친다

## 보고서

`docs/reports/N-<요약>.md` (파일명은 브랜치와 같은 규칙, 내용은 한국어). 돌아와서 고친 경우 기존 보고서에 이어 쓴다.

```markdown
# <이슈 제목> (#N)

## 작업 내용

## 변경 파일

## 테스트

## 트러블슈팅

(겪은 문제, 원인, 해결. 없으면 "없음")

## 남은 과제
```

## 끝나면

변경 요약과 보고서 경로를 보고하고 멈춘다. 커밋은 하지 않는다. 다음은 `flow-ship`.
