# Source Handling Rules

이 문서는 source_type별 원문 확보/요약 입력 원문 생성/실패 처리 규칙을 정의한다.

---

## YouTube (source_type: youtube)

### 판별
- `youtube.com/watch?v=...`
- `youtu.be/...`
- `youtube.com/shorts/...`

### 원문 우선순위
- HTML 본문 추출보다 **transcript(자막)** 을 1순위로 사용

### 저장 메타(가능하면)
- video_title, channel, published_at, duration

### Transcript 확보 전략(우선순위)
1. 공식/자동 자막: ko → 없으면 ja → en → 기타
2. 너무 길면 타임스탬프 기준 chunking 후 요약

### 폴백
1. description(+고정댓글 가능하면) 기반 요약
2. 그래도 부족하면 “요약 불가(Transcript 없음)” + 사유 기록
- 실패여도 LiteratureNote는 생성

### frontmatter 예시(권장)
- source_type: youtube
- has_transcript: true|false
- transcript_lang: ko|en|...
- failure_reason: N/A|TRANSCRIPT_NOT_AVAILABLE|TRANSCRIPT_FETCH_FAILED|VIDEO_UNAVAILABLE|REGION_OR_AGE_RESTRICTED

### 요약 포맷(권장)
- 기본 요약(500~1000자), 장문이면 1000자+
- 타임라인 하이라이트(mm:ss - 요지 5~10개)
- 핵심 주장/근거/예시
- 실무 적용 포인트
- 검증/추가 확인 질문(2~5개)

---

## X (source_type: x)

### 판별
- `https://x.com/<user>/status/<id>`
- `https://twitter.com/<user>/status/<id>`
- `https://mobile.twitter.com/<user>/status/<id>`
- (옵션) `/i/status/<id>`

### URL 정규화(중요)
- x.com → twitter.com 변환(임베드/oEmbed 호환 이슈 회피)
- UTM 등 트래킹 파라미터 제거(옵션)
- canonical: `https://twitter.com/<user>/status/<id>`

### 원문 우선순위
A) 1순위: oEmbed JSON
- `https://publish.twitter.com/oembed?url=<TWEET_URL>&omit_script=true&dnt=true&hide_thread=false`
- html의 blockquote 텍스트를 HTML→텍스트 변환해 요약 입력 원문으로 사용
- 링크/해시태그/멘션 유지

B) 2순위: 스레드 전체(옵션)
- FULL_THREAD_MODE=true일 때만 twscrape 등 인증 기반 수집

C) 최종 폴백
- 접근 제한/로그인 요구/레이트리밋 등 → 실패 사유 + 다음 액션 기록
- 실패여도 LiteratureNote는 생성

### frontmatter 예시(권장)
- source_type: x
- has_oembed: true|false
- has_thread: true|false
- post_id, author_name, author_url (가능하면)
- failure_reason: N/A|OEMBED_FETCH_FAILED|POST_UNAVAILABLE|LOGIN_REQUIRED|RATE_LIMITED|NETWORK_BLOCKED|PARSER_FAILED

### 요약 포맷(권장)
- TL;DR (최대 5줄)
- 원문 기반 요약(500~1000자, 길면 1000~1800자)
- 스레드면: 아웃라인(#1~#N) + 논지 전개 + 내 적용 아이디어 3개

---

## Speaker Deck (source_type: speakerdeck)

### 판별
- `https://speakerdeck.com/...`
- (옵션) `https://speakerdeck.com/player/...`

### 원문 우선순위
1) oEmbed 메타 + 덱 HTML의 슬라이드 텍스트
2) PDF 다운로드 가능하면 PDF→텍스트 추출
3) 둘 다 실패면 실패 로그 + 다음 액션(노트는 생성)

### oEmbed
- `https://speakerdeck.com/oembed.json?url=<DECK_URL>`
- 저장: source_title, author_name/url, provider_url, embed_html, thumbnail_url

### 실패 처리(frontmatter 권장)
- has_slide_text: true|false
- has_pdf: true|false
- slide_count (가능하면)
- failure_reason:
  - OEMBED_FETCH_FAILED
  - DECK_HTML_FETCH_FAILED
  - SLIDE_TEXT_NOT_AVAILABLE
  - PDF_DOWNLOAD_DISABLED
  - PDF_FETCH_FAILED
  - (옵션) OCR_SKIPPED

### 요약 포맷(권장)
- TL;DR (최대 5줄)
- 슬라이드 아웃라인(#1~#N)
- 핵심 주장/근거/예시
- 실무 적용 포인트
- 검증/추가 확인 질문(2~5개)

---

## Web (source_type: web)

### 원문 확보
- 웹 페이지 HTML → 본문 추출
- 가능하면 메타 추출: title, published/updated, domain
