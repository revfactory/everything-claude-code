---
name: tdd-workflow
description: 새 기능 작성, 버그 수정, 코드 리팩토링 시 이 스킬을 사용하세요. 단위, 통합, E2E 테스트를 포함한 80%+ 커버리지로 테스트 주도 개발을 강제합니다.
---

# 테스트 주도 개발 워크플로우

이 스킬은 모든 코드 개발이 TDD 원칙과 포괄적인 테스트 커버리지를 따르도록 보장합니다.

## 활성화 시점

- 새 기능이나 기능성 작성
- 버그나 이슈 수정
- 기존 코드 리팩토링
- API 엔드포인트 추가
- 새 컴포넌트 생성

## 핵심 원칙

### 1. 코드 전에 테스트
항상 테스트를 먼저 작성한 다음, 테스트를 통과시키는 코드를 구현합니다.

### 2. 커버리지 요구사항
- 최소 80% 커버리지 (단위 + 통합 + E2E)
- 모든 엣지 케이스 커버
- 오류 시나리오 테스트
- 경계 조건 검증

### 3. 테스트 타입

#### 단위 테스트
- 개별 함수와 유틸리티
- 컴포넌트 로직
- 순수 함수
- 헬퍼와 유틸리티

#### 통합 테스트
- API 엔드포인트
- 데이터베이스 작업
- 서비스 상호작용
- 외부 API 호출

#### E2E 테스트 (Playwright)
- 중요 사용자 흐름
- 완전한 워크플로우
- 브라우저 자동화
- UI 상호작용

## TDD 워크플로우 단계

### 1단계: 사용자 여정 작성
```
[역할]로서, 나는 [행동]을 하고 싶다, [이점]을 위해

예시:
사용자로서, 나는 시장을 의미적으로 검색하고 싶다,
정확한 키워드 없이도 관련 시장을 찾을 수 있도록.
```

### 2단계: 테스트 케이스 생성
각 사용자 여정에 대해 포괄적인 테스트 케이스 생성:

```typescript
describe('의미적 검색', () => {
  it('쿼리에 관련된 마켓 반환', async () => {
    // 테스트 구현
  })

  it('빈 쿼리를 우아하게 처리', async () => {
    // 엣지 케이스 테스트
  })

  it('Redis 불가 시 부분 문자열 검색으로 폴백', async () => {
    // 폴백 동작 테스트
  })

  it('유사도 점수로 결과 정렬', async () => {
    // 정렬 로직 테스트
  })
})
```

### 3단계: 테스트 실행 (실패해야 함)
```bash
npm test
# 테스트가 실패해야 함 - 아직 구현하지 않았으므로
```

### 4단계: 코드 구현
테스트를 통과시키는 최소한의 코드 작성:

```typescript
// 테스트에 의해 가이드되는 구현
export async function searchMarkets(query: string) {
  // 여기에 구현
}
```

### 5단계: 테스트 다시 실행
```bash
npm test
# 이제 테스트가 통과해야 함
```

### 6단계: 리팩토링
테스트를 그린으로 유지하면서 코드 품질 개선:
- 중복 제거
- 이름 개선
- 성능 최적화
- 가독성 향상

### 7단계: 커버리지 검증
```bash
npm run test:coverage
# 80%+ 커버리지 달성 확인
```

## 테스팅 패턴

### 단위 테스트 패턴 (Jest/Vitest)
```typescript
import { render, screen, fireEvent } from '@testing-library/react'
import { Button } from './Button'

describe('Button 컴포넌트', () => {
  it('올바른 텍스트로 렌더링', () => {
    render(<Button>클릭하세요</Button>)
    expect(screen.getByText('클릭하세요')).toBeInTheDocument()
  })

  it('클릭 시 onClick 호출', () => {
    const handleClick = jest.fn()
    render(<Button onClick={handleClick}>클릭</Button>)

    fireEvent.click(screen.getByRole('button'))

    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it('disabled prop이 true면 비활성화됨', () => {
    render(<Button disabled>클릭</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })
})
```

### API 통합 테스트 패턴
```typescript
import { NextRequest } from 'next/server'
import { GET } from './route'

describe('GET /api/markets', () => {
  it('마켓을 성공적으로 반환', async () => {
    const request = new NextRequest('http://localhost/api/markets')
    const response = await GET(request)
    const data = await response.json()

    expect(response.status).toBe(200)
    expect(data.success).toBe(true)
    expect(Array.isArray(data.data)).toBe(true)
  })

  it('쿼리 파라미터 검증', async () => {
    const request = new NextRequest('http://localhost/api/markets?limit=invalid')
    const response = await GET(request)

    expect(response.status).toBe(400)
  })

  it('데이터베이스 오류를 우아하게 처리', async () => {
    // 데이터베이스 실패 모킹
    const request = new NextRequest('http://localhost/api/markets')
    // 오류 처리 테스트
  })
})
```

### E2E 테스트 패턴 (Playwright)
```typescript
import { test, expect } from '@playwright/test'

test('사용자가 마켓을 검색하고 필터링할 수 있음', async ({ page }) => {
  // 마켓 페이지로 이동
  await page.goto('/')
  await page.click('a[href="/markets"]')

  // 페이지 로드 확인
  await expect(page.locator('h1')).toContainText('마켓')

  // 마켓 검색
  await page.fill('input[placeholder="마켓 검색"]', 'election')

  // 디바운스와 결과 대기
  await page.waitForTimeout(600)

  // 검색 결과 표시 확인
  const results = page.locator('[data-testid="market-card"]')
  await expect(results).toHaveCount(5, { timeout: 5000 })

  // 결과에 검색어 포함 확인
  const firstResult = results.first()
  await expect(firstResult).toContainText('election', { ignoreCase: true })

  // 상태로 필터링
  await page.click('button:has-text("활성")')

  // 필터된 결과 확인
  await expect(results).toHaveCount(3)
})

test('사용자가 새 마켓을 생성할 수 있음', async ({ page }) => {
  // 먼저 로그인
  await page.goto('/creator-dashboard')

  // 마켓 생성 폼 작성
  await page.fill('input[name="name"]', '테스트 마켓')
  await page.fill('textarea[name="description"]', '테스트 설명')
  await page.fill('input[name="endDate"]', '2025-12-31')

  // 폼 제출
  await page.click('button[type="submit"]')

  // 성공 메시지 확인
  await expect(page.locator('text=마켓이 성공적으로 생성되었습니다')).toBeVisible()

  // 마켓 페이지로 리다이렉트 확인
  await expect(page).toHaveURL(/\/markets\/test-market/)
})
```

## 테스트 파일 구성

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx          # 단위 테스트
│   │   └── Button.stories.tsx       # Storybook
│   └── MarketCard/
│       ├── MarketCard.tsx
│       └── MarketCard.test.tsx
├── app/
│   └── api/
│       └── markets/
│           ├── route.ts
│           └── route.test.ts         # 통합 테스트
└── e2e/
    ├── markets.spec.ts               # E2E 테스트
    ├── trading.spec.ts
    └── auth.spec.ts
```

## 외부 서비스 모킹

### Supabase 모킹
```typescript
jest.mock('@/lib/supabase', () => ({
  supabase: {
    from: jest.fn(() => ({
      select: jest.fn(() => ({
        eq: jest.fn(() => Promise.resolve({
          data: [{ id: 1, name: '테스트 마켓' }],
          error: null
        }))
      }))
    }))
  }
}))
```

### Redis 모킹
```typescript
jest.mock('@/lib/redis', () => ({
  searchMarketsByVector: jest.fn(() => Promise.resolve([
    { slug: 'test-market', similarity_score: 0.95 }
  ])),
  checkRedisHealth: jest.fn(() => Promise.resolve({ connected: true }))
}))
```

### OpenAI 모킹
```typescript
jest.mock('@/lib/openai', () => ({
  generateEmbedding: jest.fn(() => Promise.resolve(
    new Array(1536).fill(0.1) // 모킹된 1536차원 임베딩
  ))
}))
```

## 테스트 커버리지 검증

### 커버리지 리포트 실행
```bash
npm run test:coverage
```

### 커버리지 임계값
```json
{
  "jest": {
    "coverageThresholds": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80,
        "statements": 80
      }
    }
  }
}
```

## 피해야 할 일반적인 테스팅 실수

### 잘못됨: 구현 세부사항 테스트
```typescript
// 내부 상태 테스트하지 않기
expect(component.state.count).toBe(5)
```

### 올바름: 사용자에게 보이는 동작 테스트
```typescript
// 사용자가 보는 것 테스트
expect(screen.getByText('Count: 5')).toBeInTheDocument()
```

### 잘못됨: 취약한 선택자
```typescript
// 쉽게 깨짐
await page.click('.css-class-xyz')
```

### 올바름: 시맨틱 선택자
```typescript
// 변경에 강함
await page.click('button:has-text("제출")')
await page.click('[data-testid="submit-button"]')
```

### 잘못됨: 테스트 격리 없음
```typescript
// 테스트가 서로 의존함
test('사용자 생성', () => { /* ... */ })
test('같은 사용자 업데이트', () => { /* 이전 테스트에 의존 */ })
```

### 올바름: 독립적인 테스트
```typescript
// 각 테스트가 자체 데이터 설정
test('사용자 생성', () => {
  const user = createTestUser()
  // 테스트 로직
})

test('사용자 업데이트', () => {
  const user = createTestUser()
  // 업데이트 로직
})
```

## 지속적인 테스팅

### 개발 중 Watch 모드
```bash
npm test -- --watch
# 파일 변경 시 테스트가 자동 실행
```

### Pre-Commit 훅
```bash
# 모든 커밋 전 실행
npm test && npm run lint
```

### CI/CD 통합
```yaml
# GitHub Actions
- name: 테스트 실행
  run: npm test -- --coverage
- name: 커버리지 업로드
  uses: codecov/codecov-action@v3
```

## 모범 사례

1. **테스트 먼저 작성** - 항상 TDD
2. **테스트당 하나의 어설션** - 단일 동작에 집중
3. **설명적인 테스트 이름** - 무엇이 테스트되는지 설명
4. **Arrange-Act-Assert** - 명확한 테스트 구조
5. **외부 의존성 모킹** - 단위 테스트 격리
6. **엣지 케이스 테스트** - Null, undefined, 빈 값, 큰 값
7. **오류 경로 테스트** - 해피 패스만 아니라
8. **테스트를 빠르게 유지** - 단위 테스트당 50ms 미만
9. **테스트 후 정리** - 사이드 이펙트 없음
10. **커버리지 리포트 검토** - 갭 식별

## 성공 지표

- 80%+ 코드 커버리지 달성
- 모든 테스트 통과 (그린)
- 스킵되거나 비활성화된 테스트 없음
- 빠른 테스트 실행 (단위 테스트 30초 미만)
- E2E 테스트가 중요 사용자 흐름 커버
- 테스트가 프로덕션 전에 버그 포착

---

**기억하세요**: 테스트는 선택사항이 아닙니다. 테스트는 자신있는 리팩토링, 빠른 개발, 프로덕션 신뢰성을 가능하게 하는 안전망입니다.
