# Git 워크플로우

## 커밋 메시지 형식

```
<타입>: <설명>

<선택적 본문>
```

타입: feat, fix, refactor, docs, test, chore, perf, ci

참고: ~/.claude/settings.json에서 전역적으로 어트리뷰션 비활성화됨.

## Pull Request 워크플로우

PR 생성 시:
1. 전체 커밋 히스토리 분석 (최신 커밋만이 아님)
2. `git diff [base-branch]...HEAD`로 모든 변경사항 확인
3. 포괄적인 PR 요약 작성
4. TODO가 포함된 테스트 계획 포함
5. 새 브랜치인 경우 `-u` 플래그로 푸시

## 기능 구현 워크플로우

1. **먼저 계획**
   - **planner** 에이전트로 구현 계획 생성
   - 의존성과 위험 식별
   - 단계별로 분해

2. **TDD 접근법**
   - **tdd-guide** 에이전트 사용
   - 테스트 먼저 작성 (RED)
   - 테스트 통과하도록 구현 (GREEN)
   - 리팩토링 (IMPROVE)
   - 80%+ 커버리지 확인

3. **코드 리뷰**
   - 코드 작성 직후 **code-reviewer** 에이전트 사용
   - CRITICAL과 HIGH 이슈 해결
   - 가능하면 MEDIUM 이슈도 수정

4. **커밋 & 푸시**
   - 상세한 커밋 메시지
   - 컨벤셔널 커밋 형식 준수
