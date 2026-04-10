# LLM Wiki

LLM이 관리하는 개인 지식 정리 시스템. Claude Code에서 스킬로 실행합니다.

## 시작하기

### 요구사항
- Claude Max 구독 (API 불필요)
- Claude Code (코웍 claude.ai/code, 데스크톱 앱, 또는 CLI)

### 설치
1. 이 레포를 fork 또는 clone
2. Claude Code에서 이 레포를 프로젝트로 열기
3. 채팅창에서 스킬 실행 — 끝

## 스킬 사용법

`.claude/skills/` 폴더에 있는 `.md` 파일이 자동으로 스킬로 등록됩니다.
채팅창에서 `/` 입력 시 사용 가능한 스킬 목록이 나타납니다.

### 기본 제공 스킬

| 스킬 | 설명 |
|------|------|
| `/wiki-ingest` | 소스 파일/폴더를 읽고 위키에 정리 |
| `/wiki-query` | 위키 기반으로 질문에 답변 |
| `/wiki-lint` | 위키 건강 점검 |

### 사용 예시

```
/wiki-ingest raw/article.md           # 단일 파일
/wiki-ingest /home/user/notes/        # 외부 폴더 전체
/wiki-query 주요 인물 간의 관계를 정리해줘
/wiki-lint
```

### 외부 소스 폴더 접근

프로젝트 바깥 폴더를 소스로 쓰려면 `.claude/settings.json`의 `additionalDirectories`에 경로를 추가합니다:

```json
{
  "permissions": {
    "additionalDirectories": ["/home/user/notes", "/home/user/research"]
  }
}
```

## 스킬 추가/수정하는 법

### 1. 스킬 파일 작성

`.claude/skills/` 에 `.md` 파일을 만들면 자동으로 스킬로 등록됩니다.
`settings.json` 수정 불필요. 파일명이 곧 스킬 이름입니다.

```
.claude/skills/my-skill.md  →  /my-skill 으로 호출
```

스킬 파일 예시:

```markdown
# my-skill

스킬이 하는 일에 대한 설명.

## 사용법
`/my-skill [인자]`

## 워크플로우
1. 첫 번째 단계...
2. 두 번째 단계...
```

### 2. 바로 사용

파일 저장 후 채팅창에서 `/my-skill` 입력. 재시작 불필요.

### 현재 등록된 스킬

```
.claude/skills/
  wiki-ingest.md    # /wiki-ingest
  wiki-query.md     # /wiki-query
  wiki-lint.md      # /wiki-lint
```

## 프로젝트 구조

```
.claude/
  settings.json       # Claude Code 하네스 설정
  skills/             # 스킬 파일들 (자동 등록)
wiki/                 # LLM이 관리하는 정리본 (하나의 출력 폴더)
  index.md            # 전체 페이지 카탈로그
  index_summary.md    # 핵심 요약 인덱스
  log.md              # 작업 이력
  entities/           # 개체 페이지
  concepts/           # 개념 페이지
  sources/            # 소스 요약
  analysis/           # 분석/합성
CLAUDE.md             # 위키 스키마/규칙 (LLM이 매 세션 읽음)
```

## 소스 폴더

소스는 프로젝트 안팎 어디든 지정 가능합니다:
- 프로젝트 내: `raw/article.md`
- 외부 폴더: `/home/user/notes/` (additionalDirectories 설정 필요)
- 여러 소스 폴더 → `wiki/` 하나에 통합 정리
- 소스 파일은 절대 수정되지 않음 (읽기 전용)
