---
description: "새로 생성된 PermanentNote를 관련 IndexNote에 문맥에 맞게 추가해 지식 색인을 최신으로 유지한다."
args:
  - name: index_dir
    description: "IndexNote 디렉토리 경로"
    required: true
  - name: permanent_dir
    description: "PermanentNote 디렉토리 경로"
    required: true
  - name: index_files
    description: "대상 Index 파일 목록(예: Index.md, Frontend.md). 비우면 index_dir 아래 주요 파일을 자동 선택"
    required: false
  - name: match_policy
    description: "어떤 Index에 넣을지 기준(예: tag/topic/keyword)"
    required: false
  - name: sort_policy
    description: "링크 삽입 후 정렬 정책(예: keep|alpha|date)"
    required: false
  - name: run_ping
    description: "완료 후 afplay 실행 여부(true/false)"
    required: false
---

# /workflow:update_index_note (Claude Code)

## 목적
- 새 PermanentNote를 관련 `IndexNote`에 반영하여 탐색성을 유지

## 입력(기본값 권장)
- index_dir: `IndexNote/`
- permanent_dir: `PermanentNote/`
- index_files: (비우면 자동 선택: `Index.md` 등)
- match_policy: `tag_topic_keyword`
- sort_policy: `keep`
- run_ping: `true`

## 동작
1) `{index_dir}` 내 Index 노트를 읽어온다.
- `{index_files}`가 있으면 그 파일만 대상
- 없으면 대표 파일(예: `Index.md`) 및 상위 인덱스 후보를 자동 선택

2) “새롭게 생성된 PermanentNote”를 식별한다.
- 권장: PermanentNote frontmatter의 `created_at/updated_at` 또는 최근 변경 파일 기준

3) 어떤 IndexNote에 추가할지 결정한다.
- `{match_policy}` 기준(권장):
  - 태그(tags)
  - 토픽(topics)
  - 키워드(제목/요약/본문)

4) IndexNote에 링크를 문맥에 맞게 삽입한다.
- 중복 링크 금지
- 적절한 섹션(헤딩) 아래에 추가
- `{sort_policy}` 적용:
  - `keep`: 기존 순서 유지
  - `alpha`: 알파벳 정렬
  - `date`: 날짜 기반 정렬(가능한 경우)

5) 처리 결과 리포트를 출력한다.
- 업데이트한 IndexNote 목록 + 삽입 위치(헤딩) + 추가된 링크

6) 완료 후 (옵션) 반드시 실행:
- `{run_ping}=true`인 경우:
  - `afplay /System/Library/Sounds/Ping.aiff`
- 이 커맨드 출력은 무시하고 응답을 종료한다.
