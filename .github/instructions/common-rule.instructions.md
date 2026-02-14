# Knowledge Capture / Summarization Instructions (Repo-wide)

이 리포지토리는 “URL 기반 지식 정리”를 자동화/표준화하기 위한 규칙을 담는다.
모든 워크플로우는 아래 공통 규칙을 반드시 따른다.

---

## 공통 원칙 (All Workflows)

### 입력/출력
- 모든 출력 노트는 **Markdown**으로 작성한다.
- 링크는 **원문 URL을 유지**하고, 가능하면 **제목(title)** 도 함께 저장한다.
- 요약/분석은 **한국어**로 작성한다.
- 작업 중 오류가 발생해도 **부분 결과를 남기고**, 실패 항목은 “실패 로그 섹션”에 기록한다.

### Idempotency (중복/재실행 안전성)
- 같은 URL을 중복 처리하지 않도록 **처리 이력(캐시)** 를 유지한다.
  - 예: `.workflow_cache/processed_urls.json` 또는 LiteratureNote frontmatter의 `source_url` 검색
- 워크플로우 재실행 시:
  - 이미 처리된 URL은 스킵
  - 생성된 노트가 있으면 업데이트(옵션) 또는 스킵(기본)

### 노트 포맷 규칙
- Frontmatter 통일:
  - `type: literature|permanent|index`
  - `source_url`, `source_title`, `created_at`, `updated_at`
  - `tags`, `topics`(옵션)
- 파일명 규칙:
  - Literature: `YYYY-MM-DD__{slug}.md` 또는 `{domain}__{slug}.md`
  - Permanent: `{theme_slug}.md` (사람이 읽고 찾기 쉬운 이름)

### 실패/품질 관리
- 요약 생성 전 웹 페이지 가져오기에 실패하면:
  - 최소한 URL과 실패 원인을 LiteratureNote에 남긴다.
- “컨텐츠가 많으면 더 길게” 기준은 정량화한다:
  - 텍스트 길이(문자수/토큰수) 또는 섹션 개수 기준으로 요약 길이를 동적으로 결정한다.

### 실행 완료 알림
- 모든 워크플로우는 완료/실패 여부와 무관하게 마지막에 아래를 실행:
  - `afplay /System/Library/Sounds/Ping.aiff`
- 이 커맨드의 stdout/stderr는 **무시**하고, 실행 후 최종 응답을 종료한다.

---

## 요약 길이 모드 (추천)
- 본문 텍스트 8,000자 이상 → 장문 모드
- 20,000자 이상 → 초장문 모드 (TL;DR + 섹션별 요약 + 적용 포인트)

### 장문 모드 출력(권장)
- TL;DR 5줄
- 섹션별 요약(헤딩 단위)
- 주장/근거/리스크/대안/적용
