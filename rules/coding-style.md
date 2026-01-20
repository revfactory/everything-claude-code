# 코딩 스타일

## 불변성 (중요)

항상 새 객체 생성, 절대 변경하지 않음:

```javascript
// 잘못된 방법: 변경
function updateUser(user, name) {
  user.name = name  // 변경!
  return user
}

// 올바른 방법: 불변성
function updateUser(user, name) {
  return {
    ...user,
    name
  }
}
```

## 파일 구성

적은 대형 파일보다 많은 작은 파일:
- 높은 응집도, 낮은 결합도
- 일반적으로 200-400줄, 최대 800줄
- 대형 컴포넌트에서 유틸리티 추출
- 타입별이 아닌 기능/도메인별 구성

## 오류 처리

항상 포괄적인 오류 처리:

```typescript
try {
  const result = await riskyOperation()
  return result
} catch (error) {
  console.error('작업 실패:', error)
  throw new Error('상세한 사용자 친화적 메시지')
}
```

## 입력 검증

항상 사용자 입력 검증:

```typescript
import { z } from 'zod'

const schema = z.object({
  email: z.string().email(),
  age: z.number().int().min(0).max(150)
})

const validated = schema.parse(input)
```

## 코드 품질 체크리스트

작업 완료 전 확인:
- [ ] 코드가 읽기 쉽고 이름이 명확함
- [ ] 함수가 작음 (<50줄)
- [ ] 파일이 집중됨 (<800줄)
- [ ] 깊은 중첩 없음 (>4레벨)
- [ ] 적절한 오류 처리
- [ ] console.log 문 없음
- [ ] 하드코딩된 값 없음
- [ ] 변경 없음 (불변 패턴 사용)
