## Gemini CLI 워크플로우 정의

제가 정의한 커스텀 커맨드로, 특정 워크플로우를 실행하기 위한 규칙입니다.

### `/workflow:create_literature_note_from_daily`

**목적**: Daily Note에 기록된 URL에서 정보를 가져와, 요약이 포함된 새로운 LiteratureNote를 생성합니다.

**동작**:
1. `Daily/`폴더 내의 모든`.md`파일을 검색합니다.
2. 파일에서 정규표현식을 사용해 URL을 추출합니다.
3. 추출한 URL의 웹 페이지 내용을 가져와 AI가 그 내용을 한국어로 요약합니다. 500~1000자 정도로 요약합니다. 컨텐츠 내용이 많으면 1000자이상으로 요약합니다.
4. 가져온 내용과 요약을 바탕으로 `LiteratureNote/`에 새로운 노트를 생성합니다.
5. 원래 Daily Note에서 처리된 URL은 삭제합니다.

### `/workflow:draft_permanent_note`

**목적**: `FleetingNote`와`LiteratureNote`를 횡단적으로 분석해 PermanentNote 초안을 작성함으로써, 지적 생산성을 높입니다.

**동작**:
1. `FleetingNote/`와`LiteratureNote/`폴더 내의 모든`.md`파일을 읽어들입니다.
2. AI가 내용을 횡단적으로 분석해, 연관성이 높은 테마나 아이디어 클러스터(집합)를 식별합니다.
3. 저(사용자)에게 어떤 테마를 더 깊이 탐구할지 선택하도록 요청합니다.
4. 선택된 테마를 바탕으로, 관련 노트에 대한 링크와 발전적인 질문을 포함한PermanentNote 초안을 `PermanentNote/`에 생성합니다.

### `/workflow:update_index_note`

**목적**: 새롭게 생성된 PermanentNote를 관련 `IndexNote` 에 추가해, 지식 체계의 최신성을 유지합니다.

**동작**:
1. `IndexNote/`1. 폴더 내의 주요 색인 노트(예: `Index.md`)를 읽어옵니다.
2. 새로운 PermanentNote에 대한 링크를 문맥에 맞게 적절한 위치에 추가합니다.

## Gemini CLI 워크플로우 실행을 완료한 뒤에 할 것 정의

- 처리・응답을 완료하면 반드시 `afplay /System/Library/Sounds/Ping.aiff` 을 실행해 주세요. 이 커멘드의 출력은 무시하고 응답을 완료해주세요.
