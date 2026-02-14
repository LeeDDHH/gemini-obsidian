# copilot-instructions.md 내용 한국어 정리

## 1) 시스템 개요
- Obsidian Vault를 제텔카스텐(Zettelkasten) 방식으로 운영한다.
- `Daily/`에 쌓인 URL을 AI 워크플로우로 처리해 `LiteratureNote/`(요약 노트)를 만든다.
- `FleetingNote/` + `LiteratureNote/`를 분석해 `PermanentNote/`(원자적 개념 노트) 초안을 만든다.
- `PermanentNote/`를 `IndexNote/`(인덱스/맵)와 연결해 지식 네트워크를 유지한다.

## 2) 디렉터리 구조와 역할
- `Daily/`: 매일 인박스(처리할 URL을 적어두는 곳)
- `FleetingNote/`: 즉흥 아이디어/임시 메모
- `LiteratureNote/`: 외부 자료(웹/유튜브)에서 AI가 만든 요약 노트
- `PermanentNote/`: 다듬어진 핵심 개념(Atomic) 노트
- `IndexNote/`: PermanentNote를 탐색하기 위한 인덱스/맵
- `.claude/commands/`: Claude Code용 커스텀 워크플로우 정의(활성 구현 위치)
- `.cache/youtube_subs/`: 유튜브 자막 다운로드 임시 저장소
- `.workflow_cache/`: 워크플로우 처리 이력(중복 처리 방지)

## 3) 전체 파이프라인(흐름)
- `Daily/*.md (URLs)`
  → `/workflow:create_literature_note_from_daily`
  → `LiteratureNote/*.md`

- `FleetingNote/ + LiteratureNote/`
  → `/workflow:draft_permanent_note`
  → `PermanentNote/*.md`

- `PermanentNote/*.md`
  → `/workflow:update_index_note`
  → `IndexNote/*.md (updated)`

## 4) 커스텀 워크플로우 요약

### 4-1) /workflow:create_literature_note_from_daily
목적
- Daily 노트에서 URL을 추출하고, 콘텐츠를 가져와 한국어 요약을 생성해 `LiteratureNote/`에 저장한다.

핵심 동작
- URL을 YouTube/웹으로 구분한다.
  - YouTube 패턴: `youtube.com/watch`, `youtu.be`, `youtube.com/shorts`

YouTube 처리
- HTML fetch를 하지 않고 `yt-dlp`로 자막을 추출한다(비디오 다운로드 없이).
- 명령:
  - `yt-dlp --skip-download --write-subs --write-auto-subs --sub-langs "ko,ja,en" --sub-format "vtt"`
- `.vtt`를 텍스트로 변환(타임스탬프/태그/중복 제거).
- 자막 언어 우선순위: `ko → ja → en`
- 자막이 없으면 요약 없이 노트를 만들고 실패 사유를 기록한다.

Web 처리
- HTML을 가져와 본문(메인 콘텐츠)을 추출한다.

요약 길이 규칙(원문 길이에 따라 동적)
- 기본(< 8K chars): 500~1000자
- 긴 글(8K~20K chars): 1000~1800자
- 매우 긴 글(> 20K chars): 섹션 기반 구조화 요약

Daily 정리
- Daily 노트의 “단독 URL 라인”은 제거한다.
- 문맥을 위한 인라인 URL(문장 중간에 포함된 링크)은 유지한다.

재실행 안전(Idempotency)
- 처리한 URL은 `.workflow_cache/processed_urls.json`에 기록한다.
- 같은 URL을 다시 처리하지 않도록 스킵한다.
- LiteratureNote의 frontmatter `source_url`도 2차 체크로 활용한다.

완료 알림
- 완료 시 `afplay /System/Library/Sounds/Ping.aiff`를 재생한다.

### 4-2) /workflow:draft_permanent_note
목적
- `FleetingNote/`와 `LiteratureNote/` 전체를 분석해 테마를 찾고 `PermanentNote/` 초안을 만든다.

핵심 동작
- 전체 노트를 교차 분석해 3~7개 테마 클러스터를 제안한다(근거/증거 포함).
- 사용자가 테마를 선택하도록 유도한다.
- 생성되는 PermanentNote 구조:
  - TL;DR
  - Current conclusion(현재 결론)
  - Evidence / references(소스 링크 포함)
  - Counter-examples / constraints(반례/제약)
  - Practical application checklist(실천 체크리스트)
  - Follow-up questions(후속 질문)

### 4-3) /workflow:update_index_note
목적
- 새로 생성된 `PermanentNote/`를 적절한 `IndexNote/`에 추가해 탐색성을 유지한다.

핵심 동작
- `IndexNote/`를 읽는다.
- 신규 PermanentNote를 `created_at/updated_at`로 식별한다.
- 태그/토픽/키워드로 인덱스 매칭 후 해당 섹션에 링크를 삽입한다.
- 중복 링크를 방지한다.
- 기존 정렬(소트) 규칙을 유지한다.

## 5) 노트 포맷 컨벤션

### 5-1) 공통 Frontmatter(모든 노트 타입)

```
type: literature|permanent|index
source_url: <original URL>
source_title: <title>
created_at: <ISO timestamp>
updated_at: <ISO timestamp>
tags: [tag1, tag2]
topics: [topic1, topic2]
```


### 5-2) LiteratureNote 추가 Frontmatter(특히 YouTube)

```
source_type: youtube|web
has_transcript: true|false
transcript_lang: ko|ja|en|N/A
failure_reason: N/A|TRANSCRIPT_NOT_AVAILABLE|TRANSCRIPT_FETCH_FAILED|VIDEO_UNAVAILABLE|REGION_OR_AGE_RESTRICTED
```

### 5-3) 파일명 규칙
- LiteratureNote: `YYYY-MM-DD__{domain}__{slug}.md`
- PermanentNote: `{theme_slug}.md` (사람이 읽기 쉬운 설명형 이름)

### 5-4) 링크 규칙
- Obsidian 위키 링크 형식: `[[Note Title]]`
- 레퍼런스에는 원문 URL과 제목을 함께 보존한다.

## 6) 운영 원칙
- 모든 요약/분석은 한국어로 작성한다.
- 원문 제목/URL은 그대로 보존한다.
- 오류가 나도 부분 결과를 남기고 실패 사유를 기록한다.
- 재실행 시 이미 처리된 항목은 스킵한다.

## 7) 트러블슈팅 요약

### 7-1) YouTube 자막 추출 실패
1. `yt-dlp --version`으로 설치 확인
2. `pip install -U yt-dlp`로 업데이트
3. 연령 제한 영상: `yt-dlp --cookies-from-browser chrome <URL>`
4. `.cache/youtube_subs/`에 생성된 `.vtt` 확인

### 7-2) URL이 처리되지 않음
- `.workflow_cache/processed_urls.json`에 이미 존재하는지 확인(이미 처리됨)
- URL이 단독 라인 또는 마크다운 링크 형태인지 확인
- `https://` 또는 `http://` 형태인지 확인

## 8) 마이그레이션 메모
- Gemini CLI에서 Claude Code로 전환 중이다.
- 활성 워크플로우 구현은 `.claude/commands/`에 있다.
- `GEMINI.md`에는 레거시 스펙이 참고용으로 남아 있다.
