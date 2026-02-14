# Workflows

이 문서는 자동화 커맨드/워크플로우의 “무엇을/어떻게”를 요약한다.
구현 상세는 각 워크플로우 스크립트/설정 파일을 따른다.

---

## /workflow:create_literature_note_from_daily

### 목적
Daily Note에 수집된 URL을 원문 스냅샷 + 한국어 요약으로 정리해 `LiteratureNote/`에 축적하고,
Daily의 URL을 정리해 인박스 비우기를 자동화한다.

### 처리 대상
- `Daily/` 폴더 내 `**/*.md`
- URL 추출:
  - 일반 URL: `https?://...`
  - Markdown 링크: `[text](https://...)`
- 제외(권장):
  - 이미지/파일 직접 링크(.png .jpg .pdf 등) 옵션 분리
  - localhost/사내망 등 접근 불가 URL은 스킵 + 로그

### 동작(개요)
1. Daily의 모든 md에서 URL 추출
2. URL 정규화 → 중복 제거
3. source_type 판별: youtube / speakerdeck / x / web
4. 원문 확보(요약 입력 원문 생성)
5. 요약 생성(한국어, 500~1000자 기본)
6. `LiteratureNote/`에 노트 생성
7. Daily에서 처리 완료 URL 삭제(“URL 단독 라인”만)
8. 리포트 출력(성공/실패/생성 경로)

---

## /workflow:draft_permanent_note

### 목적
`FleetingNote + LiteratureNote`를 엮어 PermanentNote 초안을 빠르게 만든다.

### 동작(개요)
1. Fleeting/Literature를 읽고 반복 개념/주장/문제 추출
2. 유사도 클러스터 생성(테마 후보 3~7개)
3. 사용자 테마 선택
4. 선택 테마로 PermanentNote 초안 생성
5. `PermanentNote/` 저장

---

## /workflow:update_index_note

### 목적
새 PermanentNote를 IndexNote에 반영해 탐색성(네비)을 유지한다.

### 동작(개요)
1. IndexNote 읽기
2. 새 PermanentNote 메타/태그 기반으로 삽입 위치 추천
3. 중복 링크 금지 + 정렬 옵션 + 결과 리포트
