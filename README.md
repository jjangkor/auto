# LLM Wiki

LLM이 관리하는 개인 지식 정리 시스템. Claude Code 코웍에서 스킬로 실행합니다.

## 사용법

Claude Max 구독 → claude.ai/code 에서 이 레포를 열고 스킬 실행.

### 스킬 (슬래시 커맨드)

| 커맨드 | 설명 |
|--------|------|
| `/wiki-ingest [경로]` | 소스 파일/폴더를 읽고 위키에 정리 |
| `/wiki-query [질문]` | 위키 기반으로 질문에 답변 |
| `/wiki-lint` | 위키 건강 점검 |

### 워크플로우

1. 소스 md 파일이 있는 폴더 경로를 지정
2. `/wiki-ingest 경로` 로 인제스트 → `wiki/`에 정리본 생성
3. `/wiki-query 질문` 으로 위키 기반 질문
4. `/wiki-lint` 로 주기적 점검

### 구조

```
(소스 폴더)       # 사용자가 관리하는 md 파일들 (여러 폴더 가능)
wiki/             # LLM이 관리하는 정리본 (하나의 출력 폴더)
  index.md        # 전체 페이지 카탈로그
  index_summary.md # 핵심 요약 인덱스
  log.md          # 작업 이력
  entities/       # 개체 페이지
  concepts/       # 개념 페이지
  sources/        # 소스 요약
  analysis/       # 분석/합성
CLAUDE.md         # 위키 스키마/규칙
```

### 소스 폴더 지정

소스는 프로젝트 안팎 어디든 가능합니다:
- `raw/books/` — 프로젝트 내 폴더
- `/home/user/notes/` — PC의 다른 폴더
- 여러 소스 폴더 → `wiki/` 하나에 통합 정리
