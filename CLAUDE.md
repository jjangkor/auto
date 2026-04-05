# LLM Wiki System

이 레포는 LLM이 관리하는 개인 지식 위키입니다. 모든 위키 페이지는 LLM이 작성/유지하며, 사용자는 소스 큐레이션과 질문에 집중합니다.

## 디렉토리 구조

```
raw/            # 원본 소스 (불변, LLM이 수정하지 않음)
wiki/           # LLM이 관리하는 위키 페이지들
  index.md      # 전체 페이지 카탈로그 (카테고리별)
  index_summary.md  # 핵심 요약 인덱스 (빠른 탐색용)
  log.md        # 작업 이력 (append-only)
  overview.md   # 위키 전체 개요/합성
  entities/     # 인물, 조직, 장소 등 개체 페이지
  concepts/     # 개념, 이론, 프레임워크 페이지
  sources/      # 소스별 요약 페이지
  analysis/     # 비교, 분석, 합성 페이지
```

## 위키 페이지 형식

모든 위키 페이지는 YAML frontmatter를 포함합니다:

```markdown
---
title: 페이지 제목
type: entity | concept | source | analysis | overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [소스파일명1, 소스파일명2]
tags: [태그1, 태그2]
---

# 페이지 제목

본문 내용...

## 관련 페이지
- [[관련페이지1]] - 관계 설명
- [[관련페이지2]] - 관계 설명
```

## 크로스 레퍼런스 규칙

- `[[페이지명]]` 형식의 위키링크 사용 (Obsidian 호환)
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

1. **raw/ 는 불변** — 절대 수정하지 않음
2. **위키는 LLM이 소유** — 사용자가 직접 편집할 필요 없음
3. **모든 변경은 로깅** — log.md에 기록
4. **인덱스 항상 최신** — 페이지 생성/삭제 시 index.md, index_summary.md 동기화
5. **크로스레퍼런스 유지** — 관련 페이지 간 양방향 링크
6. **모순 명시** — 소스 간 충돌 정보는 숨기지 않고 표시
7. **합성 우선** — 단순 요약보다 기존 지식과의 통합에 집중
