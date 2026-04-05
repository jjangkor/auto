# LLM Wiki System

LLM이 관리하는 개인 지식 정리 시스템. 사용자는 소스 파일을 관리하고, LLM이 정리본을 생성/유지합니다.

## 구조

```
(소스 폴더들)    # 사용자 PC의 md 파일들 — 여러 폴더 가능, LLM은 읽기만
wiki/            # LLM이 관리하는 정리본 (하나의 출력 폴더)
  index.md       # 전체 페이지 카탈로그 (카테고리별)
  index_summary.md  # 핵심 요약 인덱스 (빠른 탐색용)
  log.md         # 작업 이력 (append-only)
  overview.md    # 전체 개요/합성
  entities/      # 인물, 조직, 장소 등 개체 페이지
  concepts/      # 개념, 이론, 프레임워크 페이지
  sources/       # 소스별 요약 페이지
  analysis/      # 비교, 분석, 합성 페이지
```

## 소스 폴더 규칙

- 소스 폴더는 프로젝트 내(`raw/`) 또는 외부 경로 어디든 가능
- 소스 파일은 **절대 수정하지 않음** (읽기 전용)
- 여러 소스 폴더의 내용이 `wiki/` 하나에 통합 정리됨

## 위키 페이지 형식

모든 위키 페이지는 YAML frontmatter를 포함합니다:

```markdown
---
title: 페이지 제목
type: entity | concept | source | analysis | overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [소스파일경로1, 소스파일경로2]
tags: [태그1, 태그2]
---

# 페이지 제목

본문 내용...

## 관련 페이지
- [[관련페이지1]] - 관계 설명
- [[관련페이지2]] - 관계 설명
```

## 크로스 레퍼런스 규칙

- `[[페이지명]]` 형식의 위키링크 사용
- 새 페이지 생성 시 관련 기존 페이지에도 역링크 추가
- 모순되는 정보 발견 시 양쪽 페이지에 명시적으로 기록

## index.md 관리

카테고리별로 모든 페이지를 정리합니다:

```markdown
## Entities
- [[entities/인물명]] - 한줄 설명 | sources: N개

## Concepts
- [[concepts/개념명]] - 한줄 설명 | sources: N개

## Sources
- [[sources/소스명]] - 한줄 설명 | date: YYYY-MM-DD

## Analysis
- [[analysis/분석명]] - 한줄 설명 | date: YYYY-MM-DD
```

## index_summary.md 관리

전체 위키의 핵심 내용을 압축 요약합니다. 주요 엔티티, 핵심 개념, 현재까지의 합성/결론을 빠르게 파악할 수 있도록 유지합니다. 새 소스 인제스트 후 필요시 업데이트합니다.

## log.md 형식

```markdown
## [YYYY-MM-DD] ingest | 소스제목
- 소스 경로: ...
- 요약: ...
- 생성/수정된 페이지: ...

## [YYYY-MM-DD] query | 질문 요약
- 답변 요약: ...
- 생성된 페이지: ... (있을 경우)

## [YYYY-MM-DD] lint
- 발견된 이슈: ...
- 수정 사항: ...
```

## 핵심 원칙

1. **소스 파일은 불변** — 어떤 경로의 소스든 절대 수정하지 않음
2. **위키는 LLM이 소유** — 사용자가 직접 편집할 필요 없음
3. **모든 변경은 로깅** — log.md에 기록
4. **인덱스 항상 최신** — 페이지 생성/삭제 시 index.md, index_summary.md 동기화
5. **크로스레퍼런스 유지** — 관련 페이지 간 양방향 링크
6. **모순 명시** — 소스 간 충돌 정보는 숨기지 않고 표시
7. **합성 우선** — 단순 요약보다 기존 지식과의 통합에 집중
