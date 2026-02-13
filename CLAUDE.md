# CLAUDE.md

이 파일은 Claude Code (claude.ai/code)가 이 저장소의 코드를 다룰 때 참고하는 가이드입니다.

## 개요

이 저장소는 제텔카스텐(Zettelkasten) 방식에서 영감을 받은 개인 지식 관리용 Obsidian 볼트이며, Claude Code 커스텀 워크플로우를 통한 AI 자동화로 강화되어 있습니다. 데일리 노트의 URL을 처리하고, 문헌 요약을 생성하고, 교차 참조된 아이디어로부터 영구 노트를 작성하며, 체계적인 지식 색인을 유지합니다.

## 아키텍처

### 디렉토리 구조

- `Daily/` - 처리할 URL이 포함된 데일리 노트
- `FleetingNote/` - 즉각적인 아이디어를 빠르게 포착하는 임시 노트
- `LiteratureNote/` - 외부 소스(웹 페이지, YouTube 영상)에서 AI로 생성한 요약 노트
- `PermanentNote/` - 핵심 개념과 아이디어를 정제한 원자적(atomic) 노트
- `IndexNote/` - 영구 노트를 탐색하기 위한 맵과 색인
- `.claude/commands/` - Claude Code 커스텀 워크플로우 정의
- `.gemini/` - Gemini CLI 설정 (레거시, Claude Code로 마이그레이션 중)
- `.cache/` - YouTube 자막 다운로드용 임시 파일
- `.workflow_cache/` - 중복 방지를 위한 워크플로우 처리 이력

## 공통 규칙 (전 워크플로우에 적용)

### 입력/출력 원칙

- 모든 출력 노트는 **Markdown**으로 작성한다.
- 링크는 원문 URL을 유지하고, 가능하면 **제목(title)** 도 함께 저장한다.
- 요약/분석은 **한국어**로 작성한다.
- 작업 중 오류가 발생해도 **부분 결과를 남기고**, 실패 항목은 "실패 로그 섹션"에 기록한다.
- Obsidian 호환을 위해 `[[위키 스타일 링크]]`를 사용한다.
- 콘텐츠는 주로 **한국어**로 작성되며, 일부 **일본어**(日本語) 노트도 포함된다.

### 중복/재실행 안전성 (Idempotency)

- 같은 URL을 중복 처리하지 않도록 **처리 이력(캐시)** 를 유지한다.
  - 예: `.workflow_cache/processed_urls.json` 또는 LiteratureNote에 `source_url` frontmatter를 두고 검색.
- 워크플로우 재실행 시:
  - 이미 처리된 URL은 스킵
  - 생성된 노트가 있으면 업데이트(옵션) 또는 스킵(기본)

### 노트 포맷 규칙

- **Frontmatter** 를 통일:
  - `type: literature|permanent|index`
  - `source_url`, `source_title`, `created_at`, `updated_at`
  - `source_type: youtube|web` (문헌 노트용)
  - `has_transcript: true|false` (YouTube 전용)
  - `transcript_lang: ko|ja|en|N/A` (YouTube 전용)
  - `failure_reason` - 처리 실패 시
  - `tags`, `topics` (옵션)
- 파일명 규칙:
  - LiteratureNote: `YYYY-MM-DD__{domain}__{slug}.md`
  - PermanentNote: `{theme_slug}.md` (사람이 읽고 찾기 쉬운 이름)

### 실패/품질 관리

- 요약 생성 전 웹 페이지 가져오기에 실패하면:
  - 최소한 URL과 실패 원인을 LiteratureNote에 남긴다.
- "컨텐츠가 많으면 더 길게"의 기준을 정량화:
  - 텍스트 길이(문자수/토큰수) 또는 섹션 개수 기준으로 요약 길이를 동적으로 결정한다.

### 실행 완료 알림

- 모든 워크플로우는 완료/실패 여부와 무관하게 마지막에 실행:
  - `afplay /System/Library/Sounds/Ping.aiff`
- 이 커맨드의 stdout/stderr는 **무시**하고, 실행 후 최종 응답을 종료한다.

### YouTube 소스 처리 규칙

#### 1) 소스 타입 판별

URL이 아래 중 하나면 `source_type: youtube`로 분류:
- `youtube.com/watch?v=...`
- `youtu.be/...`
- `youtube.com/shorts/...`

#### 2) Transcript 우선 사용

`source_type: youtube`인 경우:
- 웹페이지 본문 파싱(HTML 본문 추출)보다 transcript 텍스트를 1순위 입력으로 사용한다.
- 가능하면 함께 저장:
  - `video_title`
  - `channel`
  - `published_at` (가능하면)
  - `duration` (가능하면)

#### 3) Transcript 확보 전략 (우선순위)

1. 공식 자막/자동 생성 자막 (가능하면 한국어 → 없으면 원어)
2. 자막이 여러 언어면:
   - 우선: ko → 없으면 ja 또는 en → 그 외
3. transcript가 너무 길면:
   - 전체를 그대로 넣지 말고 타임스탬프 단위로 chunking 후 요약에 사용

**필수 도구**: `yt-dlp`로 자막 다운로드 (`--skip-download --write-subs --write-auto-subs`)

#### 4) Transcript가 없을 때 폴백

transcript를 못 구하면 아래 순서로 폴백:
1. 영상 설명(description) + 고정댓글(가능하면) 기반 요약
2. 그래도 부족하면 "요약 불가(Transcript 없음)"로 처리하고 노트에 사유 기록

"transcript 없음"이어도 LiteratureNote는 생성하되:
- TL;DR 대신 "요약 실패 사유/다음 액션(자막 켜고 재실행 등)"을 남긴다.

#### 5) 요약 포맷 (YouTube 전용)

- 기본 요약: 500~1000자, 장문이면 1000자+
- 추가 권장 섹션:
  - **타임라인 하이라이트**: `mm:ss` - 요지 5~10개
  - **핵심 주장/근거/예시**
  - **실무 적용 포인트** (내 상황에 어떻게 쓰나)
  - **검증/추가 확인 질문** (2~5개)

#### 6) LiteratureNote frontmatter 예시 (YouTube)

```yaml
source_type: youtube
source_url: ...
source_title: ...
channel: ...           # 가능하면
published_at: ...      # 가능하면
transcript_lang: ko|en|...
has_transcript: true|false
```

### 컨텐츠 양 기반 요약 규칙

#### 판정 기준

| 본문 길이 | 모드 | 요약 길이 |
|-----------|------|----------|
| ~8,000자 | 기본 | 500~1000자 |
| 8,000자 이상 | 장문 | 1000~1800자 |
| 20,000자 이상 | 초장문 | 구조화된 형식 |

#### 장문/초장문 모드 요약 포맷

- **TL;DR** 5줄
- **섹션별 요약** (헤딩 단위)
- **주장/근거/리스크/대안/적용**

## 커스텀 워크플로우

### /workflow:create_literature_note_from_daily

**목적**: 데일리 노트에서 URL을 자동 추출하고, 콘텐츠를 가져와 한국어 요약을 생성하여 문헌 노트를 만듭니다.

**주요 동작**:
- `Daily/*.md` 파일에서 URL 추출 (독립 행 또는 마크다운 링크)
- YouTube와 웹 소스를 구분하여 처리 (→ "YouTube 소스 처리 규칙" 참조)
- 요약 길이는 콘텐츠 양에 따라 동적 결정 (→ "컨텐츠 양 기반 요약 규칙" 참조)
- `.vtt` 파일을 일반 텍스트로 변환 (타임스탬프, 태그, 중복 제거)
- 처리 완료된 URL은 데일리 노트에서 제거 (문맥 보존을 위해 독립 행만 제거)

### /workflow:draft_permanent_note

**목적**: 임시 노트와 문헌 노트를 교차 분석하여 테마를 식별하고 영구 노트 초안을 작성합니다.

**주요 동작**:
- `FleetingNote/`와 `LiteratureNote/`의 모든 노트 읽기
- AI를 활용하여 반복되는 개념 추출 및 테마 클러스터 생성
- 설명과 주요 참조를 포함한 3~7개의 테마 후보 제시
- 사용자에게 테마 선택 요청
- 다음 구조의 영구 노트 생성:
  - TL;DR
  - 현재 결론
  - 근거/참고자료 (출처 링크 포함)
  - 반례/제약 조건
  - 실용적 적용 체크리스트
  - 후속 질문

### /workflow:update_index_note

**목적**: 새로 생성된 영구 노트를 관련 인덱스 노트에 추가하여 발견 가능성을 높입니다.

**주요 동작**:
- `IndexNote/*.md` 파일 읽기
- 태그/주제/키워드를 기반으로 새 영구 노트를 적절한 인덱스에 매칭
- 적절한 제목 아래에 링크 삽입
- 중복 링크 방지

## 일반 작업

### 워크플로우 실행

Skill 도구를 사용하여 커스텀 스킬을 실행합니다:
```
# 데일리 노트의 URL을 문헌 노트로 처리
/workflow:create_literature_note_from_daily

# 축적된 지식으로 영구 노트 생성
/workflow:draft_permanent_note

# 새 영구 노트로 인덱스 업데이트
/workflow:update_index_note
```

### 노트 작업 시 주의사항

- 변경하기 전에 노트를 읽어 문맥을 파악할 것
- 편집 시 프론트매터 구조를 유지할 것 (→ "노트 포맷 규칙" 참조)

### YouTube 처리 문제 해결

YouTube 자막 추출이 실패하는 경우:
1. `yt-dlp` 설치 확인: `yt-dlp --version`
2. yt-dlp 업데이트: `pip install -U yt-dlp`
3. 연령 제한 영상의 경우: `yt-dlp --cookies-from-browser chrome <URL>`
4. `.cache/youtube_subs/`에서 비디오 ID로 생성된 자막 파일 확인

## 중요 사항

- **Git 워크플로우**: 이 저장소는 표준 git 명령어를 사용합니다. 볼트에는 일반적으로 수정하지 않아야 하는 `.obsidian/` 설정이 포함되어 있습니다.
- **GEMINI.md**: Gemini CLI용 레거시 워크플로우 정의를 포함합니다. `.claude/commands/` 디렉토리에 활성 Claude Code 구현이 있습니다.

## 마이그레이션 참고

이 프로젝트는 Gemini CLI에서 Claude Code로 전환 중입니다. `GEMINI.md` 파일에는 원본 워크플로우 사양이 포함되어 있고, `.claude/commands/*.md`에는 Claude Code 구현이 포함되어 있습니다. 워크플로우 작업 시 `.claude/commands/`의 Claude Code 버전을 우선적으로 사용하세요.
