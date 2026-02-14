---
type: literature
source_url: https://speakerdeck.com/twada/working-with-legacy-code-the-true-record
source_title: 実録レガシーコード改善 / Working with Legacy Code: the True Record
created_at: 2026-02-14
updated_at: 2026-02-14
tags:
  - legacy-code
  - software-development
  - refactoring
source_type: speakerdeck
author_name: Takuto Wada
author_url: https://speakerdeck.com/twada
has_slide_text: true
has_pdf: false
slide_count: 118
---

이 일본어 텍스트는 '실제 레거시 코드 개선'이라는 주제로, 2018년 실제 수탁 개발 프로젝트의 경험을 공유하는 강연 자료의 일부입니다. 강연은 테스트 코드가 전혀 없는 레거시 코드를 인계받아 개선하는 과정을 다룹니다.

주요 내용은 다음과 같습니다:
1.  **현황 파악:** 레거시 코드의 현재 상태를 분석합니다.
2.  **負의 유산 계승:** 기술 부채를 인식하고 계승합니다.
3.  **기능 개선:** 기능 개선을 통해 수익을 창출합니다.
4.  **부채 분할:** 기술 부채를 분할하여 관리합니다.
5.  **사실과 정보 분리:** 객관적인 사실과 주관적인 정보를 구분합니다.
6.  **이상적인 아키텍처 포기:** 현실적인 제약 속에서 이상적인 아키텍처를 고집하지 않습니다.

예시로 제시된 Alexa 퀴즈 스킬은 25줄의 JavaScript로 AWS Lambda에 호스팅되어 있으며, `QuizIntent`와 `AnswerIntent`로 구성됩니다. 이 코드에는 문자열 결합 방식의 불일치나 `Attributes`의 재사용 등 레거시 코드의 특징이 나타납니다.

가장 큰 문제점은 테스트 코드의 부재, 빌드 및 배포 자동화의 미비였습니다. 마이클 피더스의 "테스트가 있다면, 개선하면서 코드의 동작을 보장할 수 있다"는 인용구를 통해 테스트 코드의 중요성을 강조합니다. 이 프로젝트는 버전 관리, 테스트, 자동화 등 지속적 배포의 필수 요소가 전혀 없는 상태에서 시작되었습니다.
