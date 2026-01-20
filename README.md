# Everything Claude Code

<p align="center">
  <img src="everything-claude-coding.png" alt="Everything Claude Code" width="800" />
</p>

**Anthropic 해커톤 우승자의 Claude Code 설정 완전 모음집.**

이 저장소에는 제가 Claude Code와 함께 매일 사용하는 프로덕션급 에이전트, 스킬, 훅, 커맨드, 규칙, MCP 설정이 담겨 있습니다. 이 설정들은 10개월 이상 실제 제품을 개발하며 발전시켜 왔습니다.

---

## 먼저 전체 가이드를 읽어보세요

**이 설정들을 살펴보기 전에 X에서 전체 가이드를 먼저 읽어보세요:**


<img width="592" height="445" alt="image" src="https://github.com/user-attachments/assets/1a471488-59cc-425b-8345-5245c7efbcef" />


**[The Shorthand Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2012378465664745795)**



이 가이드에서 설명하는 내용:
- 각 설정 유형의 역할과 사용 시점
- Claude Code 설정을 구성하는 방법
- 컨텍스트 윈도우 관리 (성능에 매우 중요)
- 병렬 워크플로우와 고급 기법
- 이 설정들의 철학

**이 저장소는 설정만 포함합니다! 팁, 트릭 및 더 많은 예제는 제 X 글과 영상에 있습니다 (발전함에 따라 이 readme에 링크가 추가될 예정입니다).**

---

## 저장소 구성

```
everything-claude-code/
|-- agents/           # 위임을 위한 전문 서브에이전트
|   |-- planner.md           # 기능 구현 계획
|   |-- architect.md         # 시스템 설계 결정
|   |-- tdd-guide.md         # 테스트 주도 개발
|   |-- code-reviewer.md     # 품질 및 보안 검토
|   |-- security-reviewer.md # 취약점 분석
|   |-- build-error-resolver.md
|   |-- e2e-runner.md        # Playwright E2E 테스트
|   |-- refactor-cleaner.md  # 불필요한 코드 정리
|   |-- doc-updater.md       # 문서 동기화
|
|-- skills/           # 워크플로우 정의 및 도메인 지식
|   |-- coding-standards.md         # 언어 모범 사례
|   |-- backend-patterns.md         # API, 데이터베이스, 캐싱 패턴
|   |-- frontend-patterns.md        # React, Next.js 패턴
|   |-- project-guidelines-example.md # 예시 프로젝트별 스킬
|   |-- tdd-workflow/               # TDD 방법론
|   |-- security-review/            # 보안 체크리스트
|   |-- clickhouse-io.md            # ClickHouse 분석
|
|-- commands/         # 빠른 실행을 위한 슬래시 커맨드
|   |-- tdd.md              # /tdd - 테스트 주도 개발
|   |-- plan.md             # /plan - 구현 계획
|   |-- e2e.md              # /e2e - E2E 테스트 생성
|   |-- code-review.md      # /code-review - 품질 검토
|   |-- build-fix.md        # /build-fix - 빌드 오류 수정
|   |-- refactor-clean.md   # /refactor-clean - 불필요한 코드 제거
|   |-- test-coverage.md    # /test-coverage - 커버리지 분석
|   |-- update-codemaps.md  # /update-codemaps - 문서 갱신
|   |-- update-docs.md      # /update-docs - 문서 동기화
|
|-- rules/            # 항상 따라야 하는 가이드라인
|   |-- security.md         # 필수 보안 점검
|   |-- coding-style.md     # 불변성, 파일 구성
|   |-- testing.md          # TDD, 80% 커버리지 요구사항
|   |-- git-workflow.md     # 커밋 형식, PR 프로세스
|   |-- agents.md           # 서브에이전트에 위임할 시점
|   |-- performance.md      # 모델 선택, 컨텍스트 관리
|   |-- patterns.md         # API 응답 형식, 훅
|   |-- hooks.md            # 훅 문서
|
|-- hooks/            # 트리거 기반 자동화
|   |-- hooks.json          # PreToolUse, PostToolUse, Stop 훅
|
|-- mcp-configs/      # MCP 서버 설정
|   |-- mcp-servers.json    # GitHub, Supabase, Vercel, Railway 등
|
|-- plugins/          # 플러그인 생태계 문서
|   |-- README.md           # 플러그인, 마켓플레이스, 스킬 가이드
|
|-- examples/         # 예시 설정
    |-- CLAUDE.md           # 예시 프로젝트 레벨 설정
    |-- user-CLAUDE.md      # 예시 사용자 레벨 설정 (~/.claude/CLAUDE.md)
    |-- statusline.json     # 커스텀 상태줄 설정
```

---

## 빠른 시작

### 1. 필요한 것을 복사

```bash
# 저장소 클론
git clone https://github.com/affaan-m/everything-claude-code.git

# 에이전트를 Claude 설정에 복사
cp everything-claude-code/agents/*.md ~/.claude/agents/

# 규칙 복사
cp everything-claude-code/rules/*.md ~/.claude/rules/

# 커맨드 복사
cp everything-claude-code/commands/*.md ~/.claude/commands/

# 스킬 복사
cp -r everything-claude-code/skills/* ~/.claude/skills/
```

### 2. hooks를 settings.json에 추가

`hooks/hooks.json`에서 훅을 복사하여 `~/.claude/settings.json`에 추가하세요.

### 3. MCP 설정

`mcp-configs/mcp-servers.json`에서 원하는 MCP 서버를 `~/.claude.json`에 복사하세요.

**중요:** `YOUR_*_HERE` 플레이스홀더를 실제 API 키로 교체하세요.

### 4. 가이드 읽기

진심으로, [가이드를 읽어보세요](https://x.com/affaanmustafa/status/2012378465664745795). 맥락을 알면 이 설정들이 10배 더 이해가 잘 됩니다.

---

## 핵심 개념

### 에이전트

서브에이전트는 제한된 범위의 위임된 작업을 처리합니다. 예시:

```markdown
---
name: code-reviewer
description: 품질, 보안, 유지보수성을 위한 코드 리뷰
tools: Read, Grep, Glob, Bash
model: opus
---

당신은 시니어 코드 리뷰어입니다...
```

### 스킬

스킬은 커맨드나 에이전트가 호출하는 워크플로우 정의입니다:

```markdown
# TDD 워크플로우

1. 먼저 인터페이스 정의
2. 실패하는 테스트 작성 (RED)
3. 최소한의 코드 구현 (GREEN)
4. 리팩토링 (IMPROVE)
5. 80%+ 커버리지 확인
```

### 훅

훅은 도구 이벤트에서 실행됩니다. 예시 - console.log 경고:

```json
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\\\.(ts|tsx|js|jsx)$\"",
  "hooks": [{
    "type": "command",
    "command": "#!/bin/bash\ngrep -n 'console\\.log' \"$file_path\" && echo '[Hook] Remove console.log' >&2"
  }]
}
```

### 규칙

규칙은 항상 따라야 하는 가이드라인입니다. 모듈화하세요:

```
~/.claude/rules/
  security.md      # 하드코딩된 시크릿 금지
  coding-style.md  # 불변성, 파일 제한
  testing.md       # TDD, 커버리지 요구사항
```

---

## 기여하기

**기여를 환영하고 권장합니다.**

이 저장소는 커뮤니티 리소스입니다. 다음을 가지고 계시다면:
- 유용한 에이전트나 스킬
- 영리한 훅
- 더 나은 MCP 설정
- 개선된 규칙

기여해 주세요! 가이드라인은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참조하세요.

### 기여 아이디어

- 언어별 스킬 (Python, Go, Rust 패턴)
- 프레임워크별 설정 (Django, Rails, Laravel)
- DevOps 에이전트 (Kubernetes, Terraform, AWS)
- 테스트 전략 (다양한 프레임워크)
- 도메인별 지식 (ML, 데이터 엔지니어링, 모바일)

---

## 배경

저는 실험적 출시 때부터 Claude Code를 사용해왔습니다. 2025년 9월 Anthropic x Forum Ventures 해커톤에서 [@DRodriguezFX](https://x.com/DRodriguezFX)와 함께 [zenith.chat](https://zenith.chat)을 만들어 우승했습니다 - 전적으로 Claude Code를 사용해서요.

이 설정들은 여러 프로덕션 애플리케이션에서 실전 테스트를 거쳤습니다.

---

## 중요 참고사항

### 컨텍스트 윈도우 관리

**매우 중요:** 모든 MCP를 한 번에 활성화하지 마세요. 200k 컨텍스트 윈도우가 너무 많은 도구를 활성화하면 70k로 줄어들 수 있습니다.

경험 법칙:
- 20-30개 MCP를 설정해두되
- 프로젝트당 10개 미만만 활성화
- 80개 미만의 도구 활성

프로젝트 설정에서 `disabledMcpServers`를 사용하여 사용하지 않는 것을 비활성화하세요.

### 커스터마이징

이 설정들은 제 워크플로우에 맞게 되어 있습니다. 다음을 권장합니다:
1. 공감되는 것부터 시작
2. 본인의 스택에 맞게 수정
3. 사용하지 않는 것 제거
4. 본인만의 패턴 추가

---

## 링크

- **전체 가이드:** [The Shorthand Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2012378465664745795)
- **팔로우:** [@affaanmustafa](https://x.com/affaanmustafa)
- **zenith.chat:** [zenith.chat](https://zenith.chat)

---

## 라이선스

MIT - 자유롭게 사용하고, 필요에 따라 수정하고, 가능하면 기여해 주세요.

---

**도움이 되셨다면 스타를 눌러주세요. 가이드를 읽으세요. 멋진 것을 만드세요.**
