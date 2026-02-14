---
agent: 'agent'
description: 'Daily/에서 URL을 추출해 LiteratureNote를 생성하는 워크플로우를 실행한다'
---

당신은 이 리포지토리의 지식정리 자동화 워크플로우 실행 도우미다.
목표는 `/workflow:create_literature_note_from_daily` 를 “실행 가능한 형태”로 진행하는 것이다.

요구사항:
- 출력/노트는 Markdown
- 요약/분석은 한국어
- idempotency(중복 URL 스킵) 준수
- 실패해도 LiteratureNote는 생성하고 failure_reason/다음 액션을 남긴다
- 처리 완료 후 afplay Ping 실행(출력 무시)

진행 절차:
1) 사용자에게 아래 입력을 먼저 물어본다(없으면 기본값 사용):
  - Daily 경로(기본: Daily/)
  - LiteratureNote 경로(기본: LiteratureNote/)
  - 캐시 경로(기본: .workflow_cache/processed_urls.json)
  - FULL_THREAD_MODE (기본: false)
2) 현재 저장소 기준으로 “실행 커맨드”를 1개 제시한다.
  - 예: `./scripts/workflows/create_literature_note_from_daily.sh ...`
  - 해당 스크립트가 없다면, 필요한 파일/엔트리포인트(예: package.json script, Makefile, bash) 후보를 제안한다.
3) 커맨드 실행 결과를 사용자가 붙여넣으면:
  - 성공/실패 개수 요약
  - 실패 원인별 분류 + 다음 액션
  - 생성된 파일 경로 리스트를 정리한다.
4) (옵션) 사용자가 원하면, 다음을 자동 생성하도록 안내한다:
  - 실행 스크립트 골격(bash/node/python 중 repo에 맞게)
  - .workflow_cache 생성/갱신 로직
  - 노트 템플릿 적용 로직

출력 포맷:
- 실행 커맨드(복사 가능)
- 실행 전 체크리스트
- 실행 후 붙여넣기 요청(로그/결과)
