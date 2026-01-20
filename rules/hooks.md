# 훅 시스템

## 훅 타입

- **PreToolUse**: 도구 실행 전 (검증, 파라미터 수정)
- **PostToolUse**: 도구 실행 후 (자동 포맷, 검사)
- **Stop**: 세션 종료 시 (최종 검증)

## 현재 훅 (~/.claude/settings.json 내)

### PreToolUse
- **tmux 리마인더**: 장시간 명령에 tmux 제안 (npm, pnpm, yarn, cargo 등)
- **git push 리뷰**: 푸시 전 Zed에서 리뷰 열기
- **doc 블로커**: 불필요한 .md/.txt 파일 생성 차단

### PostToolUse
- **PR 생성**: PR URL과 GitHub Actions 상태 로깅
- **Prettier**: 편집 후 JS/TS 파일 자동 포맷
- **TypeScript 검사**: .ts/.tsx 파일 편집 후 tsc 실행
- **console.log 경고**: 편집된 파일의 console.log 경고

### Stop
- **console.log 감사**: 세션 종료 전 모든 수정된 파일의 console.log 검사

## 자동 승인 권한

주의해서 사용:
- 신뢰할 수 있고 잘 정의된 계획에 활성화
- 탐색적 작업에는 비활성화
- dangerously-skip-permissions 플래그 절대 사용 금지
- 대신 `~/.claude.json`에서 `allowedTools` 설정

## TodoWrite 모범 사례

TodoWrite 도구 사용 용도:
- 다단계 작업 진행 상황 추적
- 지시사항 이해도 확인
- 실시간 조정 가능
- 세분화된 구현 단계 표시

할 일 목록으로 알 수 있는 것:
- 순서가 맞지 않는 단계
- 누락된 항목
- 불필요한 추가 항목
- 잘못된 세분화 수준
- 잘못 해석된 요구사항
