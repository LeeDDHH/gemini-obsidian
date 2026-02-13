---
description: "Daily/의 URL들을 처리해 LiteratureNote/를 생성한다. YouTube는 yt-dlp로 transcript를 우선 추출해 요약한다."
args:
  - name: daily_dir
    description: "Daily 노트 디렉토리 경로"
    required: true
  - name: literature_dir
    description: "LiteratureNote 디렉토리 경로"
    required: true
  - name: cache_dir
    description: "캐시 디렉토리 경로(.cache 등)"
    required: false
  - name: url_regex
    description: "URL 추출용 정규표현식(기본값 사용 가능)"
    required: false
  - name: yt_langs
    description: "YouTube 자막 언어 우선순위 (예: ko,ja,en)"
    required: false
  - name: delete_policy
    description: "Daily에서 URL 삭제 정책 (only_standalone_line 권장)"
    required: false
  - name: summary_len_default
    description: "기본 요약 길이(문자 수) 범위. 예: 500-1000"
    required: false
  - name: summary_len_long
    description: "장문 요약 길이(문자 수) 범위. 예: 1000-1800"
    required: false
  - name: long_threshold_chars
    description: "장문 판정 기준(원문 문자 수). 예: 8000"
    required: false
  - name: very_long_threshold_chars
    description: "초장문 판정 기준(원문 문자 수). 예: 20000"
    required: false
  - name: run_ping
    description: "완료 후 afplay 실행 여부(true/false)"
    required: false
---

# /workflow:create_literature_note_from_daily (Claude Code)

## 목적
- Daily Note에 수집된 URL을 **원문 스냅샷 + 한국어 요약**으로 정리해 `LiteratureNote/`에 축적
- Daily의 URL을 정리하여 **인박스 비우기** 자동화

## 입력(기본값 권장)
- daily_dir: `Daily/`
- literature_dir: `LiteratureNote/`
- cache_dir: `.cache/`
- yt_langs: `ko,ja,en`
- delete_policy: `only_standalone_line`
- summary_len_default: `500-1000`
- summary_len_long: `1000-1800`
- long_threshold_chars: `8000`
- very_long_threshold_chars: `20000`
- run_ping: `true`

## 처리 대상
- `{daily_dir}/**/*.md`
- URL 추출 대상
  - 일반 URL: `https?://...`
  - Markdown 링크: `[text](https://...)`
- 제외 규칙(권장)
  - 이미지/파일 직접 링크(`.png .jpg .pdf` 등) → 스킵 + 로그
  - `localhost`, 사내망 등 접근 불가 URL → 스킵 + 로그

## 동작
1) `{daily_dir}` 내 모든 `.md`를 찾고, 파일별로 URL 목록을 추출한다.
- URL 정규화(UTM 제거 옵션, 말미 `/` 정리 등) 후 중복 제거한다.

2) URL별 `source_type`을 판별한다.
- youtube:
  - `youtube.com/watch?v=...`
  - `youtu.be/...`
  - `youtube.com/shorts/...`
- web: 그 외

3) 각 URL에 대해 “요약 입력 원문”을 확보한다.

### 3-A) source_type: web

- 웹 페이지 본문을 가져온다(HTML → 본문 추출).
- 가능하면 메타를 추출한다: 제목(title), 발행일/업데이트일, 도메인.

### 3-B) source_type: youtube (옵션 A: yt-dlp, HTML fetch 금지)
- 원칙:
  - YouTube는 **HTML fetch(웹페이지 본문 추출)를 시도하지 않는다**(차단 빈도 높음).
  - **transcript(자막) 텍스트**를 “요약 입력 원문”으로 사용한다.

- 자막 추출(영상 다운로드 X, 자막만):
```bash
yt-dlp \
  --skip-download \
  --write-subs --write-auto-subs \
  --sub-langs "{yt_langs}" \
  --sub-format "vtt" \
  -o "{cache_dir}/youtube_subs/%(id)s.%(ext)s" \
  "<YOUTUBE_URL>"
```

- .vtt 선택 규칙:
  - 언어 우선순위: ko → ja → en → (기타)
- .vtt → plain text 변환 규칙:
  - 제거: 타임스탬프/큐 번호/태그/빈 줄
  - 중복 라인 축약
- 성공 시 메타 기록:
  - source_type: youtube
  - has_transcript: true
  - transcript_lang: <선택 언어>

### 3-B-FAIL) transcript 실패 시
- .vtt 미생성/확보 실패면:
  - has_transcript: false
  - transcript_lang: N/A
  - failure_reason:
    - TRANSCRIPT_NOT_AVAILABLE | TRANSCRIPT_FETCH_FAILED | VIDEO_UNAVAILABLE | REGION_OR_AGE_RESTRICTED
- 이 경우에도 LiteratureNote는 생성하되:
  - TL;DR에 “요약 실패 사유/다음 액션”을 작성한다.
  - 다음 액션 예:
    - 자막 유무 확인 후 재시도
    - 필요 시 쿠키 기반 재시도: yt-dlp --cookies-from-browser chrome ...

4. 요약 생성(한국어)
- 기본: {summary_len_default} (문자 수 기준)
- 장문(원문 {long_threshold_chars} 이상): {summary_len_long}
- 초장문(원문 {very_long_threshold_chars} 이상): 아래 형태로 구조화
  - ① 5줄 TL;DR
  - ② 섹션별 요약(헤딩/주제 단위)
  - ③ 핵심 주장/근거/리스크/적용 포인트

5. {literature_dir}에 새 노트를 생성한다.

- 파일명 권장: YYYY-MM-DD__{domain}__{slug_or_id}.md

6. Daily Note에서 처리 완료된 URL만 제거한다.

- {delete_policy}=only_standalone_line:
  - “URL만 있는 단독 라인” 또는 “링크만 있는 bullet”만 제거
  - 문장 중간에 섞인 URL은 제거하지 않는다(문맥 보호)

7. 처리 결과 리포트를 출력한다.

- 성공 N개, 실패 N개, 생성 파일 경로 리스트

8. 완료 후 (옵션) 반드시 실행:

- {run_ping}=true인 경우:
  - afplay /System/Library/Sounds/Ping.aiff
- 이 커맨드 출력은 무시하고 응답을 종료한다.

LiteratureNote 템플릿
- Frontmatter(기본)
  - type: literature
  - source_url
  - source_title
  - created_at
  - tags
- Frontmatter(추가 권장)
  - source_type: youtube|web
  - has_transcript: true|false
  - transcript_lang: ko|ja|en|N/A
  - failure_reason: N/A|TRANSCRIPT_NOT_AVAILABLE|TRANSCRIPT_FETCH_FAILED|VIDEO_UNAVAILABLE|REGION_OR_AGE_RESTRICTED
- 섹션
  - TL;DR (3~5줄)
  - 요약
  - 핵심 포인트 / 인용(필요 시)
  - 내 적용 아이디어(선택)
- 메타(가져온 시간, 파서/모드 등)
