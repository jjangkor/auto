# LLM Wiki

LLM이 관리하는 개인 지식 정리 시스템. Claude Code 코웍에서 스킬로 실행합니다.

## 시작하기

### 요구사항
- Claude Max 구독 (API 불필요)
- claude.ai/code (코웍) 또는 Claude Code CLI

### 설치
1. 이 레포를 fork하거나 clone
2. claude.ai/code 에서 이 레포를 프로젝트로 열기
3. 채팅창에 `/wiki-ingest 소스경로` 입력 — 끝

## 스킬 사용법

코웍 채팅창에서 `/` 를 입력하면 등록된 스킬 목록이 나타납니다.

| 커맨드 | 설명 |
|--------|------|
| `/wiki-ingest [경로]` | 소스 파일/폴더를 읽고 위키에 정리 |
| `/wiki-query [질문]` | 위키 기반으로 질문에 답변 |
| `/wiki-lint` | 위키 건강 점검 |

### 예시

```
# 단일 파일 인제스트
/wiki-ingest raw/article.md

# 폴더 전체 인제스트 (미처리 파일만)
/wiki-ingest /home/user/notes/

# 위키에 질문
/wiki-query 주요 인물 간의 관계를 정리해줘

# 위키 점검
/wiki-lint
```

## 스킬 추가/수정하는 법

스킬은 `.claude/skills/` 폴더의 마크다운 파일 + `.claude/settings.json` 등록으로 구성됩니다.

### 1. 스킬 파일 작성

`.claude/skills/` 에 마크다운 파일을 만듭니다:

```markdown
# my-skill

스킬이 하는 일에 대한 설명.

## 사용법
`/my-skill [인자]`

## 워크플로우
1. 첫 번째 단계...
2. 두 번째 단계...
```

### 2. settings.json에 등록

`.claude/settings.json` 의 `skills` 항목에 추가합니다:

```json
{
  "skills": {
    "my-skill": {
      "description": "스킬 한줄 설명",
      "path": ".claude/skills/my-skill.md"
    }
  }
}
```

### 3. 바로 사용

코웍 채팅창에서 `/my-skill` 로 실행됩니다. 재시작 필요 없음.

### 현재 등록된 스킬 파일

```
.claude/
  settings.json           # 스킬 등록 목록
  skills/
    wiki-ingest.md        # /wiki-ingest 스킬 정의
    wiki-query.md         # /wiki-query 스킬 정의
    wiki-lint.md          # /wiki-lint 스킬 정의
```

## 워크플로우

1. 소스 md 파일이 있는 폴더 경로를 지정
2. `/wiki-ingest 경로` 로 인제스트 → `wiki/`에 정리본 생성
3. `/wiki-query 질문` 으로 위키 기반 질문
4. `/wiki-lint` 로 주기적 점검

## 구조

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

## 소스 폴더 지정

소스는 프로젝트 안팎 어디든 가능합니다:
- `raw/books/` — 프로젝트 내 폴더
- `/home/user/notes/` — PC의 다른 폴더
- 여러 소스 폴더 → `wiki/` 하나에 통합 정리
