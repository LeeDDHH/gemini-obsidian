# Gemini Obsidian 지식 관리 Vault

이 저장소는 Zettelkasten 방식에서 영감을 받은 원칙에 따라 개인 지식 관리를 간소화하도록 설계된 맞춤형 Gemini CLI와 통합된 Obsidian Vault를 포함합니다. Obsidian의 노트 작성 및 정리 기능을 활용하고, Gemini CLI를 통한 자동화된 워크플로우를 통해 정보 처리 및 통찰력 생성을 강화합니다.

## 기능

이 Vault에는 지식 관리 프로세스를 자동화하고 향상시키기 위한 맞춤형 Gemini CLI 워크플로우가 포함되어 있습니다:

-   **`/workflow:create_literature_note_from_daily`**: Daily Notes에서 URL을 자동으로 추출하고, 웹 페이지 콘텐츠를 가져오고, AI를 사용하여 요약하고, 새로운 Literature Notes를 생성하고, 처리된 URL을 Daily Note에서 정리합니다.
-   **`/workflow:draft_permanent_note`**: Fleeting Notes와 Literature Notes를 분석하여 관련 테마와 아이디어를 식별하고, 테마 선택을 돕고, 관련 노트에 대한 링크와 생각을 자극하는 질문이 포함된 Permanent Note 초안을 작성합니다.
-   **`/workflow:update_index_note`**: 새로 생성된 Permanent Notes에 대한 링크를 메인 Index Note에 자동으로 추가하여 지식 기반을 체계적으로 유지합니다.

## 디렉토리 구조

-   `Daily/`: 일일 노트. 종종 초기 생각과 캡처된 URL을 포함합니다.
-   `FleetingNote/`: 즉각적인 아이디어를 캡처하는, 정제되지 않은 빠른 노트.
-   `LiteratureNote/`: 외부 소스(책, 기사, 웹 페이지)의 요약 및 통찰력.
-   `PermanentNote/`: 핵심 개념과 아이디어를 나타내는 정제된 원자적 노트.
-   `IndexNote/`: Permanent Notes 및 지식 기반을 탐색하기 위한 맵 및 인덱스.
-   `.gemini/`: Gemini CLI 구성 파일.
-   `.obsidian/`: Obsidian의 구성 파일.

## 시작하기

### 전제 조건

-   [Obsidian](https://obsidian.md/) (데스크톱 애플리케이션)
-   Gemini CLI (이 프로젝트는 Gemini CLI가 이 Vault와 상호 작용하도록 설정되어 있다고 가정합니다.)

### 설치

1.  **이 저장소를 복제합니다:**
    ```bash
    git clone https://github.com/your-username/gemini-obsidian.git
    cd gemini-obsidian
    ```
2.  **Obsidian에서 Vault를 엽니다:**
    -   Obsidian을 엽니다.
    -   "다른 Vault 열기" -> "폴더를 Vault로 열기"를 클릭합니다.
    -   방금 복제한 `gemini-obsidian` 디렉토리로 이동하여 선택합니다.
3.  **Gemini CLI 구성:**
    -   Gemini CLI가 이 디렉토리를 가리키는지 확인합니다. `.gemini/settings.json` 파일은 기본 구성을 위해 이미 존재합니다. `GEMINI.md`에 정의된 사용자 지정 워크플로우를 인식하고 실행하도록 Gemini CLI의 환경 또는 구성을 조정해야 할 수 있습니다.

## 사용법

설정이 완료되면 수동 노트 작성에는 Obsidian을 사용하고, 자동화된 작업에는 Gemini CLI를 사용하여 지식 기반과 상호 작용할 수 있습니다.

워크플로우를 트리거하려면 Gemini CLI 환경 내에서 사용자 지정 명령을 사용합니다:

-   `workflow:create_literature_note_from_daily`
-   `workflow:draft_permanent_note`
-   `workflow:update_index_note`

(이러한 사용자 지정 명령을 실행하는 방법에 대한 자세한 내용은 Gemini CLI 설명서를 참조하십시오.)

## 기여

기여를 환영합니다! 새로운 워크플로우, 개선 사항 또는 버그 수정에 대한 아이디어가 있다면 이슈를 열거나 풀 리퀘스트를 제출하십시오.

## 라이선스

[선택 사항: 여기에 라이선스 정보를 추가하십시오. 예: MIT 라이선스]
