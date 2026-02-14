# Note Templates

---

## LiteratureNote Template (기본)

```yaml
type: literature
source_type: web
source_url: ""
source_title: ""
created_at: ""
updated_at: ""
tags: []
topics: []
```

### TL;DR
- (3~5줄)

### 요약
- (500~1000자, 장문이면 1000~1800자)

### 핵심 포인트 / 인용(필요 시)
- ...

### 내 적용 아이디어(선택)
- ...

### 메타
- fetched_at:
- parser/mode:
- failure_log: (있으면)

---

## LiteratureNote Template (YouTube)

```yaml
type: literature
source_type: youtube
source_url: ""
source_title: ""
channel: ""
published_at: ""
duration: ""
created_at: ""
updated_at: ""
tags: []
topics: []
has_transcript: true
transcript_lang: "ko"
failure_reason: "N/A"
```

### TL;DR
- (3~5줄)

### 타임라인 하이라이트
- 00:00 - ...
- 00:00 - ...

### 요약
- (500~1000자, 장문이면 1000자+)

### 핵심 주장 / 근거 / 예시
- 주장:
- 근거:
- 예시:

### 실무 적용 포인트
- ...

### 검증/추가 확인 질문
- ...

### 메타
- fetched_at:
- transcript_source: (official|auto|description_fallback)
- failure_log: (있으면)

---

## LiteratureNote Template (X / Twitter)

```yaml
type: literature
source_type: x
source_url: ""
source_title: ""
author_name: ""
author_url: ""
post_id: ""
created_at: ""
updated_at: ""
tags: []
topics: []
has_oembed: true
has_thread: false
failure_reason: "N/A"
```

### TL;DR
- (최대 5줄)

### 원문(추출 텍스트)
> ...

### 요약
- (500~1000자, 길면 1000~1800자)

### 스레드 아웃라인(스레드일 때)
- #1 ...
- #2 ...
- #N ...

### 논지 전개(주장→근거→예시→결론)
- 주장:
- 근거:
- 예시:
- 결론:

### 내 적용 아이디어(실무)
- 1)
- 2)
- 3)

### 메타
- fetched_at:
- oembed_endpoint:
- thread_mode: (off|twscrape)
- failure_log: (있으면)

---

## LiteratureNote Template (Speaker Deck)

```yaml
type: literature
source_type: speakerdeck
source_url: ""
source_title: ""
author_name: ""
author_url: ""
slide_count: ""
created_at: ""
updated_at: ""
tags: []
topics: []
has_slide_text: true
has_pdf: false
failure_reason: "N/A"
```

### TL;DR
- (최대 5줄)

### 슬라이드 아웃라인
- #1 ...
- #2 ...
- #N ...

### 요약
- (500~1000자, 장문이면 1000자+)

### 핵심 주장 / 근거 / 예시
- 주장:
- 근거:
- 예시:

### 실무 적용 포인트
- ...

### 검증/추가 확인 질문
- ...

### 메타
- fetched_at:
- oembed_endpoint:
- has_pdf:
- failure_log: (있으면)

---

## PermanentNote Template

```yaml
type: permanent
theme: ""
created_at: ""
updated_at: ""
tags: []
topics: []
source_notes: []
```

### TL;DR
- (3~5줄)

### 내 결론(현재 시점)
- ...

### 근거(참조 링크)
- - [ ] [[LiteratureNote/...]]
- - [ ] [[FleetingNote/...]]
- - [ ] URL: ...

### 반례/제약
- ...

### 실무 적용 체크리스트
- [ ] ...
- [ ] ...

### 다음 질문(업데이트 트리거)
- ...

---

## IndexNote Template

```yaml
type: index
title: ""
created_at: ""
updated_at: ""
tags: []
topics: []
```

### 목적/범위
- ...

### 링크(주제별)
#### Topic A
- [[PermanentNote/...]]
- [[LiteratureNote/...]]

#### Topic B
- [[PermanentNote/...]]
- [[LiteratureNote/...]]
