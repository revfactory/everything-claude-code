# 예시 프로젝트 CLAUDE.md

이것은 예시 프로젝트 레벨 CLAUDE.md 파일입니다. 프로젝트 루트에 배치하세요.

## 프로젝트 개요

[프로젝트에 대한 간략한 설명 - 무엇을 하는지, 기술 스택]

## 중요 규칙

### 1. 코드 구성

- 적은 대형 파일보다 많은 작은 파일
- 높은 응집도, 낮은 결합도
- 일반적으로 200-400줄, 파일당 최대 800줄
- 타입별이 아닌 기능/도메인별 구성

### 2. 코드 스타일

- 코드, 주석, 문서에 이모지 금지
- 항상 불변성 - 객체나 배열을 절대 변경하지 않음
- 프로덕션 코드에 console.log 금지
- try/catch로 적절한 오류 처리
- Zod 또는 유사한 도구로 입력 검증

### 3. 테스팅

- TDD: 테스트 먼저 작성
- 최소 80% 커버리지
- 유틸리티에 단위 테스트
- API에 통합 테스트
- 중요 흐름에 E2E 테스트

### 4. 보안

- 하드코딩된 시크릿 금지
- 민감한 데이터에 환경 변수
- 모든 사용자 입력 검증
- 파라미터화된 쿼리만 사용
- CSRF 보호 활성화

## 파일 구조

```
src/
|-- app/              # Next.js 앱 라우터
|-- components/       # 재사용 가능한 UI 컴포넌트
|-- hooks/            # 커스텀 React 훅
|-- lib/              # 유틸리티 라이브러리
|-- types/            # TypeScript 정의
```

## 주요 패턴

### API 응답 형식

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
}
```

### 오류 처리

```typescript
try {
  const result = await operation()
  return { success: true, data: result }
} catch (error) {
  console.error('작업 실패:', error)
  return { success: false, error: '사용자 친화적 메시지' }
}
```

## 환경 변수

```bash
# 필수
DATABASE_URL=
API_KEY=

# 선택
DEBUG=false
```

## 사용 가능한 커맨드

- `/tdd` - 테스트 주도 개발 워크플로우
- `/plan` - 구현 계획 생성
- `/code-review` - 코드 품질 검토
- `/build-fix` - 빌드 오류 수정

## Git 워크플로우

- 컨벤셔널 커밋: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`
- main에 직접 커밋 금지
- PR은 리뷰 필요
- 머지 전 모든 테스트 통과 필수
