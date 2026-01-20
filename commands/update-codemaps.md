# 코드맵 업데이트

코드베이스 구조를 분석하고 아키텍처 문서 업데이트:

1. 모든 소스 파일의 임포트, 익스포트, 의존성 스캔
2. 다음 형식으로 토큰 효율적인 코드맵 생성:
   - codemaps/architecture.md - 전체 아키텍처
   - codemaps/backend.md - 백엔드 구조
   - codemaps/frontend.md - 프론트엔드 구조
   - codemaps/data.md - 데이터 모델 및 스키마

3. 이전 버전과의 차이 비율 계산
4. 변경이 30% 초과 시 업데이트 전 사용자 승인 요청
5. 각 코드맵에 최신 타임스탬프 추가
6. .reports/codemap-diff.txt에 리포트 저장

분석에 TypeScript/Node.js 사용. 구현 세부사항이 아닌 고수준 구조에 집중.
