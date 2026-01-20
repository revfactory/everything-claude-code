---
description: Playwright로 end-to-end 테스트를 생성하고 실행합니다. 테스트 여정 생성, 테스트 실행, 스크린샷/비디오/트레이스 캡처, 아티팩트 업로드.
---

# E2E 커맨드

이 커맨드는 **e2e-runner** 에이전트를 호출하여 Playwright를 사용한 end-to-end 테스트를 생성, 유지보수, 실행합니다.

## 이 커맨드가 하는 일

1. **테스트 여정 생성** - 사용자 흐름을 위한 Playwright 테스트 생성
2. **E2E 테스트 실행** - 여러 브라우저에서 테스트 실행
3. **아티팩트 캡처** - 실패 시 스크린샷, 비디오, 트레이스
4. **결과 업로드** - HTML 리포트 및 JUnit XML
5. **불안정한 테스트 식별** - 불안정한 테스트 격리

## 사용 시점

`/e2e` 사용 시점:
- 중요 사용자 여정 테스트 (로그인, 거래, 결제)
- 멀티 스텝 흐름이 end-to-end로 작동하는지 확인
- UI 상호작용 및 네비게이션 테스트
- 프론트엔드와 백엔드 간 통합 검증
- 프로덕션 배포 준비

## 작동 방식

e2e-runner 에이전트가 수행하는 작업:

1. 사용자 흐름 **분석** 및 테스트 시나리오 식별
2. Page Object Model 패턴으로 **Playwright 테스트 생성**
3. 여러 브라우저에서 **테스트 실행** (Chrome, Firefox, Safari)
4. 스크린샷, 비디오, 트레이스로 **실패 캡처**
5. 결과 및 아티팩트가 포함된 **리포트 생성**
6. **불안정한 테스트 식별** 및 수정 권장

## 테스트 아티팩트

테스트 실행 시 캡처되는 아티팩트:

**모든 테스트에서:**
- 타임라인과 결과가 포함된 HTML 리포트
- CI 통합을 위한 JUnit XML

**실패 시에만:**
- 실패 상태의 스크린샷
- 테스트의 비디오 녹화
- 디버깅을 위한 트레이스 파일 (단계별 재생)
- 네트워크 로그
- 콘솔 로그

## 아티팩트 보기

```bash
# 브라우저에서 HTML 리포트 보기
npx playwright show-report

# 특정 트레이스 파일 보기
npx playwright show-trace artifacts/trace-abc123.zip

# 스크린샷은 artifacts/ 디렉토리에 저장됨
open artifacts/search-results.png
```

## 브라우저 설정

테스트는 기본적으로 여러 브라우저에서 실행:
- ✅ Chromium (Desktop Chrome)
- ✅ Firefox (Desktop)
- ✅ WebKit (Desktop Safari)
- ✅ Mobile Chrome (선택사항)

`playwright.config.ts`에서 브라우저 조정 설정.

## 모범 사례

**해야 할 것:**
- ✅ 유지보수성을 위해 Page Object Model 사용
- ✅ 선택자에 data-testid 속성 사용
- ✅ 임의의 타임아웃 대신 API 응답 대기
- ✅ 중요 사용자 여정을 end-to-end로 테스트
- ✅ main 머지 전 테스트 실행
- ✅ 테스트 실패 시 아티팩트 검토

**하지 말아야 할 것:**
- ❌ 불안정한 선택자 사용 (CSS 클래스는 변경될 수 있음)
- ❌ 구현 세부사항 테스트
- ❌ 프로덕션에서 테스트 실행
- ❌ 불안정한 테스트 무시
- ❌ 실패 시 아티팩트 검토 건너뛰기
- ❌ E2E로 모든 엣지 케이스 테스트 (단위 테스트 사용)

## 중요 참고사항

**치명적:**
- 실제 돈이 관련된 E2E 테스트는 반드시 테스트넷/스테이징에서만 실행
- 프로덕션에서 거래 테스트 절대 금지
- 금융 테스트에 `test.skip(process.env.NODE_ENV === 'production')` 설정
- 소액 테스트 자금이 있는 테스트 지갑만 사용

## 다른 커맨드와의 통합

- `/plan`을 사용하여 테스트할 중요 여정 식별
- `/tdd`를 사용하여 단위 테스트 (더 빠르고 세분화됨)
- `/e2e`를 사용하여 통합 및 사용자 여정 테스트
- `/code-review`를 사용하여 테스트 품질 확인

## 관련 에이전트

이 커맨드는 다음 에이전트를 호출합니다:
`~/.claude/agents/e2e-runner.md`

## 빠른 명령어

```bash
# 모든 E2E 테스트 실행
npx playwright test

# 특정 테스트 파일 실행
npx playwright test tests/e2e/markets/search.spec.ts

# headed 모드로 실행 (브라우저 표시)
npx playwright test --headed

# 테스트 디버그
npx playwright test --debug

# 테스트 코드 생성
npx playwright codegen http://localhost:3000

# 리포트 보기
npx playwright show-report
```
