# 공통 패턴

## API 응답 형식

```typescript
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
```

## 커스텀 훅 패턴

```typescript
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => setDebouncedValue(value), delay)
    return () => clearTimeout(handler)
  }, [value, delay])

  return debouncedValue
}
```

## 레포지토리 패턴

```typescript
interface Repository<T> {
  findAll(filters?: Filters): Promise<T[]>
  findById(id: string): Promise<T | null>
  create(data: CreateDto): Promise<T>
  update(id: string, data: UpdateDto): Promise<T>
  delete(id: string): Promise<void>
}
```

## 스켈레톤 프로젝트

새로운 기능 구현 시:
1. 검증된 스켈레톤 프로젝트 검색
2. 병렬 에이전트로 옵션 평가:
   - 보안 평가
   - 확장성 분석
   - 관련성 점수
   - 구현 계획
3. 최적의 매치를 기반으로 클론
4. 검증된 구조 내에서 반복 개발
