---
type: literature
source_url: https://iret.media/134907
source_title: CotEditorのスクリプトを活用してみる(Mac) | iret.media
created_at: 2026-02-14
updated_at: 2025-01-19
tags:
  - coteditor
  - mac
  - script
  - python
  - json
source_type: web
domain: iret.media
---

이 글은 Mac용 텍스트 편집기인 CotEditor의 스크립트 기능을 활용하여 JSON 텍스트를 포맷하는 방법을 소개한다. 윈도우 환경에서 EmEditor의 매크로 기능에 익숙했던 저자가 Mac으로 전환하면서 CotEditor에서 유사한 기능을 구현하려는 시도에서 시작되었다.

**CotEditor 스크립트 기능 활용:**
1.  **스크립트 가이드 확인:** CotEditor의 도움말 메뉴에서 'CotEditor 스크립트 가이드'를 통해 'UNIX 스크립트와의 연동' 섹션을 참조하여 Python 스크립트 예제를 확인한다.
2.  **스크립트 파일 배치:** '스크립트 폴더 열기' 메뉴를 통해 스크립트 파일을 저장할 폴더를 열고 Python 파일을 배치한다.
3.  **스크립트 파일명 규칙:**
    *   `숫자)` 접두사를 사용하여 스크립트 메뉴에서의 표시 순서를 지정할 수 있다 (예: `01)test.py`).
    *   `.수정키.단축키`를 확장자 직전에 추가하여 키보드 단축키를 할당할 수 있다 (예: `test.@1.py`는 `Command + 1`로 실행).
4.  **Python 스크립트 작성:**
    *   첫 줄에 `#!/usr/bin/env python` 또는 `#!/usr/bin/env python3`와 같이 Shebang을 명시하여 Python 실행 경로를 지정한다.
    *   `%%%{CotEditorXInput=Selection}%%%` 및 `%%%{CotEditorXOutput=ReplaceSelection}%%%`과 같은 주석을 사용하여 스크립트 입력(현재 선택 텍스트 또는 전체 텍스트 등)과 출력(선택 텍스트 교체, 새 문서 생성 등) 방식을 설정한다.
    *   `sys.stdin`으로 입력을 받고 `print`로 출력하는 방식으로 Python 로직을 작성한다.

**JSON 포맷 스크립트 예제:**
저자는 선택된 텍스트를 JSON으로 파싱하고 들여쓰기 2칸으로 포맷하여 다시 출력하는 Python 스크립트 (`01)format_json.@1.py`)를 직접 작성했다. 이 스크립트는 `Command + 1` 단축키로 실행된다.

**결론:**
스크립트 기능을 통해 CotEditor의 활용도를 높일 수 있음을 확인했다. 특히 공식 문서를 잘 읽는 것이 중요하다고 강조하며, 생성형 AI를 활용하면 이러한 스크립트를 쉽게 만들 수 있어 앞으로도 다양한 스크립트를 시도해 볼 계획이다.
