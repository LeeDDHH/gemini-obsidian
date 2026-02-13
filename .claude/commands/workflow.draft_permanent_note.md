---
description: "FleetingNote/와 LiteratureNote/를 횡단 분석해 테마 클러스터를 만들고, 선택된 테마로 PermanentNote 초안을 생성한다."
args:
  - name: fleeting_dir
    description: "FleetingNote 디렉토리 경로"
    required: true
  - name: literature_dir
    description: "LiteratureNote 디렉토리 경로"
    required: true
  - name: permanent_dir
    description: "PermanentNote 디렉토리 경로"
    required: true
  - name: scope
    description: "분석 범위(예: all|recent_30d|changed_only)"
    required: false
  - name: max_themes
    description: "테마 후보 최대 개수(예: 7)"
    required: false
  - name: run_ping
    description: "완료 후 afplay 실행 여부(true/false)"
    required: false
---

# /workflow:draft_permanent_note (Claude Code)

## 목적
- `FleetingNote/` + `LiteratureNote/`를 횡단 분석하여 **PermanentNote 초안**을 작성
- 지식의 연결/축적을 가속

## 입력(기본값 권장)
- fleeting_dir: `FleetingNote/`
- literature_dir: `LiteratureNote/`
- permanent_dir: `PermanentNote/`
- scope: `recent_30d` (노트가 많다면)
- max_themes: `7`
- run_ping: `true`

## 동작
1) `{fleeting_dir}`, `{literature_dir}`의 `.md`를 읽는다.
- `{scope}` 적용:
  - `all`: 전체
  - `recent_30d`: 최근 30일 생성/수정
  - `changed_only`: 마지막 실행 이후 변경분(캐시 필요)

2) AI가 횡단 분석한다.
- 반복 등장 개념/문제/주장 추출
- 유사도 기반으로 **테마/아이디어 클러스터** 생성
- 각 클러스터에 포함:
  - 테마명
  - 왜 중요한지(1~2줄)
  - 근거 노트 링크 TOP 3~5

3) 사용자에게 테마 선택을 요청한다.
- 출력 형식: “후보 3~{max_themes}개” + 한줄 설명 + 대표 링크
- 사용자가 1개(또는 여러 개) 선택하도록 유도

4) 선택된 테마로 PermanentNote 초안을 생성한다.
- 포함 요소:
  - TL;DR
  - 내 결론(현재 시점)
  - 근거/참조 링크(출처 분리)
  - 반례/제약/리스크
  - 실무 적용 체크리스트
  - 다음 질문/다음 행동(PoC/실험)

5) `{permanent_dir}`에 저장한다.
- 파일명 권장: `{theme_slug}.md` (중복 시 suffix)

6) 처리 결과 리포트를 출력한다.
- 생성된 PermanentNote 경로
- 참고한 노트 리스트(선택)

7) 완료 후 (옵션) 반드시 실행:
- `{run_ping}=true`인 경우:
  - `afplay /System/Library/Sounds/Ping.aiff`
- 이 커맨드 출력은 무시하고 응답을 종료한다.
