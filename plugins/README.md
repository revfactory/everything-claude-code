# 플러그인과 마켓플레이스

플러그인은 새로운 도구와 기능으로 Claude Code를 확장합니다. 이 가이드는 설치만 다룹니다 - 언제, 왜 사용하는지는 [전체 글](https://x.com/affaanmustafa/status/2012378465664745795)을 참조하세요.

---

## 마켓플레이스

마켓플레이스는 설치 가능한 플러그인의 저장소입니다.

### 마켓플레이스 추가

```bash
# 공식 Anthropic 마켓플레이스 추가
claude plugin marketplace add https://github.com/anthropics/claude-plugins-official

# 커뮤니티 마켓플레이스 추가
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep
```

### 추천 마켓플레이스

| 마켓플레이스 | 소스 |
|--------------|------|
| claude-plugins-official | `anthropics/claude-plugins-official` |
| claude-code-plugins | `anthropics/claude-code` |
| Mixedbread-Grep | `mixedbread-ai/mgrep` |

---

## 플러그인 설치

```bash
# 플러그인 브라우저 열기
/plugins

# 또는 직접 설치
claude plugin install typescript-lsp@claude-plugins-official
```

### 추천 플러그인

**개발:**
- `typescript-lsp` - TypeScript 인텔리전스
- `pyright-lsp` - Python 타입 검사
- `hookify` - 대화형으로 훅 생성
- `code-simplifier` - 코드 리팩토링

**코드 품질:**
- `code-review` - 코드 리뷰
- `pr-review-toolkit` - PR 자동화
- `security-guidance` - 보안 검사

**검색:**
- `mgrep` - 향상된 검색 (ripgrep보다 좋음)
- `context7` - 라이브 문서 조회

**워크플로우:**
- `commit-commands` - Git 워크플로우
- `frontend-design` - UI 패턴
- `feature-dev` - 기능 개발

---

## 빠른 설정

```bash
# 마켓플레이스 추가
claude plugin marketplace add https://github.com/anthropics/claude-plugins-official
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep

# /plugins 열고 필요한 것 설치
```

---

## 플러그인 파일 위치

```
~/.claude/plugins/
|-- cache/                    # 다운로드된 플러그인
|-- installed_plugins.json    # 설치된 목록
|-- known_marketplaces.json   # 추가된 마켓플레이스
|-- marketplaces/             # 마켓플레이스 데이터
```
