# 보안 가이드라인

## 필수 보안 검사

모든 커밋 전:
- [ ] 하드코딩된 시크릿 없음 (API 키, 비밀번호, 토큰)
- [ ] 모든 사용자 입력 검증됨
- [ ] SQL 인젝션 방지 (파라미터화된 쿼리)
- [ ] XSS 방지 (HTML 새니타이즈)
- [ ] CSRF 보호 활성화
- [ ] 인증/인가 검증됨
- [ ] 모든 엔드포인트에 레이트 리미팅
- [ ] 오류 메시지가 민감한 데이터 노출하지 않음

## 시크릿 관리

```typescript
// 절대 금지: 하드코딩된 시크릿
const apiKey = "sk-proj-xxxxx"

// 항상: 환경 변수
const apiKey = process.env.OPENAI_API_KEY

if (!apiKey) {
  throw new Error('OPENAI_API_KEY가 설정되지 않았습니다')
}
```

## 보안 대응 프로토콜

보안 이슈 발견 시:
1. 즉시 중단
2. **security-reviewer** 에이전트 사용
3. 계속하기 전에 CRITICAL 이슈 수정
4. 노출된 시크릿 로테이션
5. 유사한 이슈가 있는지 전체 코드베이스 검토
