# LLM Wiki

LLM이 관리하는 개인 지식 위키 시스템.

## 사용법 (Claude Code)

Claude Max 구독으로 claude.ai/code 또는 Claude Code CLI에서 실행합니다. API 불필요.

### 스킬 (슬래시 커맨드)

| 커맨드 | 설명 |
|--------|------|
| `/wiki-ingest` | 소스를 위키에 인제스트 |
| `/wiki-query` | 위키에 질문 |
| `/wiki-lint` | 위키 건강 점검 |

### 워크플로우

1. `raw/` 에 소스 파일(마크다운, 텍스트 등)을 넣는다
2. `/wiki-ingest raw/파일명.md` 로 인제스트
3. `/wiki-query 궁금한 것` 으로 질문
4. `/wiki-lint` 로 주기적 점검

### 구조

```
raw/              # 원본 소스 (불변)
wiki/             # LLM이 관리하는 위키
  index.md        # 전체 페이지 카탈로그
  index_summary.md # 핵심 요약 인덱스
  log.md          # 작업 이력
  entities/       # 개체 페이지
  concepts/       # 개념 페이지
  sources/        # 소스 요약
  analysis/       # 분석/합성
CLAUDE.md         # 위키 스키마/규칙
```

### Obsidian 연동

이 레포를 Obsidian vault로 열면 `[[위키링크]]`와 그래프 뷰가 작동합니다.
