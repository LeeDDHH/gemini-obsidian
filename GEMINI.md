## Gemini CLI 워크플로우 정의

제가 정의한 커스텀 커맨드로, 특정 워크플로우를 실행하기 위한 규칙입니다.

##  **공통 규칙 (전 워크플로우에 적용)**

### **입력/출력 원칙**

- 모든 출력 노트는 **Markdown**으로 작성한다.
- 링크는 원문 URL을 유지하고, 가능하면 **제목(title)** 도 함께 저장한다.
- 요약/분석은 **한국어**로 작성한다.
- 작업 중 오류가 발생해도 **부분 결과를 남기고**, 실패 항목은 “실패 로그 섹션”에 기록한다.

### **중복/재실행 안전성 (Idempotency)**

- 같은 URL을 중복 처리하지 않도록 **처리 이력(캐시)** 를 유지한다.
  - 예: .workflow_cache/processed_urls.json 또는 LiteratureNote에 source_url frontmatter를 두고 검색.
- 워크플로우 재실행 시:
  - 이미 처리된 URL은 스킵
  - 생성된 노트가 있으면 업데이트(옵션) 또는 스킵(기본)

### **노트 포맷 규칙**

- **Frontmatter** 를 통일:
  - type: literature|permanent|index
  - source_url, source_title, created_at, updated_at
  - tags, topics(옵션)
- 파일명 규칙:
  - Literature: YYYY-MM-DD__{slug}.md 또는 {domain}__{slug}.md
  - Permanent: {theme_slug}.md (사람이 읽고 찾기 쉬운 이름)

### **실패/품질 관리**

- 요약 생성 전 웹 페이지 가져오기에 실패하면:
  - 최소한 URL과 실패 원인을 LiteratureNote에 남긴다.
- “컨텐츠가 많으면 더 길게”의 기준을 정량화:
  - 텍스트 길이(문자수/토큰수) 또는 섹션 개수 기준으로 요약 길이를 동적으로 결정한다.

### **실행 완료 알림**

- 모든 워크플로우는 완료/실패 여부와 무관하게 마지막에 아래를 실행:
  - afplay /System/Library/Sounds/Ping.aiff
- 이 커맨드의 stdout/stderr는 **무시**하고, 실행 후 최종 응답을 종료한다.

## `/workflow:create_literature_note_from_daily`

### 목적

Daily Note에 수집된 URL을 원문 스냅샷 + 한국어 요약으로 정리해 LiteratureNote/에 축적하고, Daily의 URL은 정리하여 인박스 비우기를 자동화한다.

### 처리 대상
- Daily/ 폴더 내 **/*.md
- URL 추출 대상:
  - 일반 URL: https?://...
  - Markdown 링크: [text](https://...)
- 제외 규칙(권장):
  - 이미지/파일 직접 링크(.png .jpg .pdf 등)는 옵션으로 분리 처리
  - localhost, 사내망 등 접근 불가 URL은 스킵 + 로그

### 동작
1. Daily/의 모든 .md 파일을 검색하고, 파일별로 URL 목록을 추출한다.
2. URL을 정규화한다(말미 /, UTM 제거 옵션 등) → 중복 제거.
3. 각 URL을 가져온다(HTML → 본문 추출).
  - 가능하면 제목(title), 발행일/업데이트일, 도메인을 함께 추출한다.
4. 요약 생성(한국어):
  - 기본: 500~1000자
  - “컨텐츠가 많음” 판정 시: 1000~1800자 (상한 권장)
  - “컨텐츠가 매우 많음”이면:
    - ① 5줄 TL;DR
    - ② 섹션별 요약(헤딩 단위)
    - ③ 핵심 주장/근거/리스크/적용 포인트
    - 형태로 구조화한다.
5. LiteratureNote/에 새 노트를 생성한다.
6. Daily Note에서 처리 완료된 URL만 제거한다.
  - 단, 원문 문맥이 깨질 수 있으니 “URL만 단독 라인일 때만 삭제” 같은 안전 규칙을 둔다(권장).
7. 처리 결과를 요약 리포트로 출력한다(성공 N개, 실패 N개, 생성 파일 경로).

### 생성되는 LiteratureNote 템플릿
- Frontmatter: type: literature, source_url, source_title, created_at, tags
- 섹션:
- TL;DR (3~5줄)
- 요약(요구 길이)
- 핵심 포인트 / 인용(필요 시)
- 내 적용 아이디어(선택)
- 메타(가져온 시간, 파서/모드 등)


## `/workflow:draft_permanent_note`

### 목적

FleetingNote + LiteratureNote를 가로로 엮어 지식의 “내 언어” 정리(PermanentNote) 초안을 빠르게 만든다.

### 동작
1. FleetingNote/, LiteratureNote/의 모든 .md를 읽는다.
  - 너무 많으면 최근 N일 / 변경분만 / 태그 필터 옵션 제공(권장).
2. AI가 다음을 수행:
  - 노트들에서 반복 등장 개념/주장/문제를 추출
  - 유사도 기반으로 클러스터(테마 묶음) 생성
  - 각 클러스터에: “이 테마가 왜 중요한가”, “근거가 되는 노트 링크 TOP5”를 붙인다.
3. 사용자에게 테마 선택을 요청:
  - 출력은 “테마 후보 3~7개 + 한 줄 설명 + 대표 노트 링크” 정도로 짧게.
4. 선택된 테마로 PermanentNote 초안 생성:
  - 정의/핵심 주장/근거/반례/적용 가이드/남는 질문
  - 관련 노트 링크(참조/근거로 명확히 분리)
  - 다음 행동(실험/PoC/리서치 질문)
5. PermanentNote/에 생성한다.
  - 파일명은 테마 slug 기반(중복 시 버전 suffix)

### PermanentNote 템플릿
  - TL;DR
  - 내 결론(현재 시점)
  - 근거(참조 링크)
  - 반례/제약
  - 실무 적용 체크리스트
  - 다음 질문(업데이트 트리거)

## `/workflow:update_index_note`

## 목적

새 PermanentNote를 적절한 IndexNote에 반영해 탐색성(네비게이션)을 유지한다.

## 동작
1. IndexNote/ 내 주요 색인 노트를 읽는다(예: Index.md, 또는 여러 개).
2. 새 PermanentNote의 메타/태그/토픽을 기반으로:
  - 어느 IndexNote에 들어가야 하는지 자동 추천
  - 문맥에 맞는 섹션(헤딩) 아래에 링크를 추가
3. 링크 삽입은 다음 규칙:
  - 기존 항목과 중복 링크 금지
  - 알파벳/날짜 정렬 옵션 제공
  - 추가한 위치를 결과로 리포트

### `/workflow:create_literature_note_from_daily_old`

**목적**: Daily Note에 기록된 URL에서 정보를 가져와, 요약이 포함된 새로운 LiteratureNote를 생성합니다.

**동작**:
1. `Daily/`폴더 내의 모든`.md`파일을 검색합니다.
2. 파일에서 정규표현식을 사용해 URL을 추출합니다.
3. 추출한 URL의 웹 페이지 내용을 가져와 AI가 그 내용을 한국어로 요약합니다. 500~1000자 정도로 요약합니다. 컨텐츠 내용이 많으면 1000자이상으로 요약합니다.
4. 가져온 내용과 요약을 바탕으로 `LiteratureNote/`에 새로운 노트를 생성합니다.
5. 원래 Daily Note에서 처리된 URL은 삭제합니다.

### `/workflow:draft_permanent_note_old`

**목적**: `FleetingNote`와`LiteratureNote`를 횡단적으로 분석해 PermanentNote 초안을 작성함으로써, 지적 생산성을 높입니다.

**동작**:
1. `FleetingNote/`와`LiteratureNote/`폴더 내의 모든`.md`파일을 읽어들입니다.
2. AI가 내용을 횡단적으로 분석해, 연관성이 높은 테마나 아이디어 클러스터(집합)를 식별합니다.
3. 저(사용자)에게 어떤 테마를 더 깊이 탐구할지 선택하도록 요청합니다.
4. 선택된 테마를 바탕으로, 관련 노트에 대한 링크와 발전적인 질문을 포함한PermanentNote 초안을 `PermanentNote/`에 생성합니다.

### `/workflow:update_index_note_old`

**목적**: 새롭게 생성된 PermanentNote를 관련 `IndexNote` 에 추가해, 지식 체계의 최신성을 유지합니다.

**동작**:
1. `IndexNote/`1. 폴더 내의 주요 색인 노트(예: `Index.md`)를 읽어옵니다.
2. 새로운 PermanentNote에 대한 링크를 문맥에 맞게 적절한 위치에 추가합니다.


## 실행 완료 후 알림(공통)
- 워크플로우 처리 종료 시(성공/실패 무관) 반드시:
- afplay /System/Library/Sounds/Ping.aiff
- 이 커맨드의 출력은 무시하고 종료한다.

## “컨텐츠 양이 많으면 자세히”를 더 정확하게 만드는 추천 규칙
- 판정 기준(예시)
- 본문 텍스트가 8,000자 이상 → 장문 모드
- 20,000자 이상 → 초장문 모드(섹션 요약 + TL;DR + 적용 포인트)
- 장문 모드 요약 포맷(권장)
- TL;DR 5줄
- 섹션별 요약(헤딩 단위)
- 주장/근거/리스크/대안/적용
