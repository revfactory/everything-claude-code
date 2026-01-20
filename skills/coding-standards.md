---
name: coding-standards
description: TypeScript, JavaScript, React, Node.js 개발을 위한 범용 코딩 표준, 모범 사례, 패턴.
---

# 코딩 표준 & 모범 사례

모든 프로젝트에 적용 가능한 범용 코딩 표준.

## 코드 품질 원칙

### 1. 가독성 우선
- 코드는 작성보다 읽히는 횟수가 더 많음
- 명확한 변수와 함수 이름
- 주석보다 자기 문서화 코드 선호
- 일관된 포맷팅

### 2. KISS (Keep It Simple, Stupid)
- 작동하는 가장 단순한 해결책
- 과도한 엔지니어링 피하기
- 조기 최적화 금지
- 이해하기 쉬운 코드 > 영리한 코드

### 3. DRY (Don't Repeat Yourself)
- 공통 로직을 함수로 추출
- 재사용 가능한 컴포넌트 생성
- 모듈 간 유틸리티 공유
- 복사-붙여넣기 프로그래밍 피하기

### 4. YAGNI (You Aren't Gonna Need It)
- 필요하기 전에 기능을 만들지 않기
- 추측성 일반화 피하기
- 필요할 때만 복잡성 추가
- 단순하게 시작하고 필요시 리팩토링

## TypeScript/JavaScript 표준

### 변수 명명

```typescript
// 좋음: 설명적인 이름
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000

// 나쁨: 불명확한 이름
const q = 'election'
const flag = true
const x = 1000
```

### 함수 명명

```typescript
// 좋음: 동사-명사 패턴
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// 나쁨: 불명확하거나 명사만
async function market(id: string) { }
function similarity(a, b) { }
function email(e) { }
```

### 불변성 패턴 (중요)

```typescript
// 항상 스프레드 연산자 사용
const updatedUser = {
  ...user,
  name: 'New Name'
}

const updatedArray = [...items, newItem]

// 절대 직접 변경하지 않기
user.name = 'New Name'  // 나쁨
items.push(newItem)     // 나쁨
```

### 오류 처리

```typescript
// 좋음: 포괄적인 오류 처리
async function fetchData(url: string) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }

    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    throw new Error('데이터 조회 실패')
  }
}

// 나쁨: 오류 처리 없음
async function fetchData(url) {
  const response = await fetch(url)
  return response.json()
}
```

### Async/Await 모범 사례

```typescript
// 좋음: 가능하면 병렬 실행
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])

// 나쁨: 불필요한 순차 실행
const users = await fetchUsers()
const markets = await fetchMarkets()
const stats = await fetchStats()
```

### 타입 안전성

```typescript
// 좋음: 적절한 타입
interface Market {
  id: string
  name: string
  status: 'active' | 'resolved' | 'closed'
  created_at: Date
}

function getMarket(id: string): Promise<Market> {
  // 구현
}

// 나쁨: 'any' 사용
function getMarket(id: any): Promise<any> {
  // 구현
}
```

## React 모범 사례

### 컴포넌트 구조

```typescript
// 좋음: 타입이 있는 함수형 컴포넌트
interface ButtonProps {
  children: React.ReactNode
  onClick: () => void
  disabled?: boolean
  variant?: 'primary' | 'secondary'
}

export function Button({
  children,
  onClick,
  disabled = false,
  variant = 'primary'
}: ButtonProps) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {children}
    </button>
  )
}

// 나쁨: 타입 없음, 불명확한 구조
export function Button(props) {
  return <button onClick={props.onClick}>{props.children}</button>
}
```

### 커스텀 훅

```typescript
// 좋음: 재사용 가능한 커스텀 훅
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)

    return () => clearTimeout(handler)
  }, [value, delay])

  return debouncedValue
}

// 사용 예
const debouncedQuery = useDebounce(searchQuery, 500)
```

### 상태 관리

```typescript
// 좋음: 적절한 상태 업데이트
const [count, setCount] = useState(0)

// 이전 상태 기반 업데이트시 함수형 업데이트
setCount(prev => prev + 1)

// 나쁨: 직접 상태 참조
setCount(count + 1)  // 비동기 시나리오에서 오래된 값일 수 있음
```

### 조건부 렌더링

```typescript
// 좋음: 명확한 조건부 렌더링
{isLoading && <Spinner />}
{error && <ErrorMessage error={error} />}
{data && <DataDisplay data={data} />}

// 나쁨: 삼항 연산자 지옥
{isLoading ? <Spinner /> : error ? <ErrorMessage error={error} /> : data ? <DataDisplay data={data} /> : null}
```

## API 설계 표준

### REST API 규칙

```
GET    /api/markets              # 모든 마켓 목록
GET    /api/markets/:id          # 특정 마켓 조회
POST   /api/markets              # 새 마켓 생성
PUT    /api/markets/:id          # 마켓 업데이트 (전체)
PATCH  /api/markets/:id          # 마켓 업데이트 (부분)
DELETE /api/markets/:id          # 마켓 삭제

# 필터링을 위한 쿼리 파라미터
GET /api/markets?status=active&limit=10&offset=0
```

### 응답 형식

```typescript
// 좋음: 일관된 응답 구조
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: {
    total: number
    page: number
    limit: number
  }
}

// 성공 응답
return NextResponse.json({
  success: true,
  data: markets,
  meta: { total: 100, page: 1, limit: 10 }
})

// 오류 응답
return NextResponse.json({
  success: false,
  error: '잘못된 요청'
}, { status: 400 })
```

### 입력 검증

```typescript
import { z } from 'zod'

// 좋음: 스키마 검증
const CreateMarketSchema = z.object({
  name: z.string().min(1).max(200),
  description: z.string().min(1).max(2000),
  endDate: z.string().datetime(),
  categories: z.array(z.string()).min(1)
})

export async function POST(request: Request) {
  const body = await request.json()

  try {
    const validated = CreateMarketSchema.parse(body)
    // 검증된 데이터로 진행
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({
        success: false,
        error: '검증 실패',
        details: error.errors
      }, { status: 400 })
    }
  }
}
```

## 파일 구성

### 프로젝트 구조

```
src/
├── app/                    # Next.js App Router
│   ├── api/               # API 라우트
│   ├── markets/           # 마켓 페이지
│   └── (auth)/           # 인증 페이지 (라우트 그룹)
├── components/            # React 컴포넌트
│   ├── ui/               # 범용 UI 컴포넌트
│   ├── forms/            # 폼 컴포넌트
│   └── layouts/          # 레이아웃 컴포넌트
├── hooks/                # 커스텀 React 훅
├── lib/                  # 유틸리티와 설정
│   ├── api/             # API 클라이언트
│   ├── utils/           # 헬퍼 함수
│   └── constants/       # 상수
├── types/                # TypeScript 타입
└── styles/              # 전역 스타일
```

### 파일 명명

```
components/Button.tsx          # 컴포넌트는 PascalCase
hooks/useAuth.ts              # 'use' 접두사가 붙은 camelCase
lib/formatDate.ts             # 유틸리티는 camelCase
types/market.types.ts         # .types 접미사가 붙은 camelCase
```

## 주석 & 문서화

### 언제 주석을 달아야 하는가

```typescript
// 좋음: 무엇이 아닌 왜를 설명
// 장애 시 API 과부하를 피하기 위해 지수 백오프 사용
const delay = Math.min(1000 * Math.pow(2, retryCount), 30000)

// 대량 배열에서 성능을 위해 의도적으로 변경 사용
items.push(newItem)

// 나쁨: 명백한 것 설명
// 카운터를 1 증가
count++

// 이름을 사용자 이름으로 설정
name = user.name
```

### 공개 API용 JSDoc

```typescript
/**
 * 의미적 유사성을 사용하여 마켓을 검색합니다.
 *
 * @param query - 자연어 검색 쿼리
 * @param limit - 최대 결과 수 (기본값: 10)
 * @returns 유사도 점수로 정렬된 마켓 배열
 * @throws {Error} OpenAI API 실패 또는 Redis 불가 시
 *
 * @example
 * ```typescript
 * const results = await searchMarkets('election', 5)
 * console.log(results[0].name) // "Trump vs Biden"
 * ```
 */
export async function searchMarkets(
  query: string,
  limit: number = 10
): Promise<Market[]> {
  // 구현
}
```

## 성능 모범 사례

### 메모이제이션

```typescript
import { useMemo, useCallback } from 'react'

// 좋음: 비용이 많이 드는 계산 메모이제이션
const sortedMarkets = useMemo(() => {
  return markets.sort((a, b) => b.volume - a.volume)
}, [markets])

// 좋음: 콜백 메모이제이션
const handleSearch = useCallback((query: string) => {
  setSearchQuery(query)
}, [])
```

### 지연 로딩

```typescript
import { lazy, Suspense } from 'react'

// 좋음: 무거운 컴포넌트 지연 로딩
const HeavyChart = lazy(() => import('./HeavyChart'))

export function Dashboard() {
  return (
    <Suspense fallback={<Spinner />}>
      <HeavyChart />
    </Suspense>
  )
}
```

### 데이터베이스 쿼리

```typescript
// 좋음: 필요한 컬럼만 선택
const { data } = await supabase
  .from('markets')
  .select('id, name, status')
  .limit(10)

// 나쁨: 모든 것 선택
const { data } = await supabase
  .from('markets')
  .select('*')
```

## 테스트 표준

### 테스트 구조 (AAA 패턴)

```typescript
test('유사도를 올바르게 계산', () => {
  // Arrange (준비)
  const vector1 = [1, 0, 0]
  const vector2 = [0, 1, 0]

  // Act (실행)
  const similarity = calculateCosineSimilarity(vector1, vector2)

  // Assert (검증)
  expect(similarity).toBe(0)
})
```

### 테스트 명명

```typescript
// 좋음: 설명적인 테스트 이름
test('쿼리와 일치하는 마켓이 없으면 빈 배열 반환', () => { })
test('OpenAI API 키가 없으면 오류 발생', () => { })
test('Redis 불가 시 부분 문자열 검색으로 폴백', () => { })

// 나쁨: 모호한 테스트 이름
test('works', () => { })
test('test search', () => { })
```

## 코드 스멜 감지

다음 안티패턴을 주의하세요:

### 1. 긴 함수
```typescript
// 나쁨: 50줄 초과 함수
function processMarketData() {
  // 100줄 코드
}

// 좋음: 작은 함수로 분리
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}
```

### 2. 깊은 중첩
```typescript
// 나쁨: 5+ 레벨 중첩
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // 무언가 수행
        }
      }
    }
  }
}

// 좋음: 조기 반환
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return

// 무언가 수행
```

### 3. 매직 넘버
```typescript
// 나쁨: 설명 없는 숫자
if (retryCount > 3) { }
setTimeout(callback, 500)

// 좋음: 명명된 상수
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500

if (retryCount > MAX_RETRIES) { }
setTimeout(callback, DEBOUNCE_DELAY_MS)
```

**기억하세요**: 코드 품질은 협상 불가입니다. 명확하고 유지보수 가능한 코드는 빠른 개발과 자신있는 리팩토링을 가능하게 합니다.
