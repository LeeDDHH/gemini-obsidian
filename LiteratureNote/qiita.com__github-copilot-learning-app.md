---
type: literature
source_url: https://qiita.com/ricecountry/items/f486f9ebfd1416af3f5c
source_title: GitHub Copilotに「GitHub Copilot学習アプリ」を作ってもらった話 #AI - Qiita
created_at: 2026-02-14
updated_at: 2026-02-14
tags:
  - github-copilot
  - AI-development
  - nextjs
  - vs-code-extension
source_type: web
domain: qiita.com
---

이 글은 필자가 GitHub Copilot을 활용하여 'Copilot Skill Builder'라는 학습 앱을 개발한 경험과 그 과정에서 얻은 지식을 공유한다. 필자는 Copilot 활용에 대한 막연함과 체계적인 학습 기회의 부족을 해결하고자, Copilot 스스로 Copilot 사용법을 배우는 앱을 만들게 되었다고 설명한다.

애플리케이션은 Next.js 14 (App Router), TypeScript, Prisma + SQLite, NextAuth.js v5를 기반으로 한 웹 앱과 VS Code 확장 기능으로 구성된다. 웹 앱에서는 코스 및 미션 관리, 진행 상황 추적, 인증 및 API를 제공하며, VS Code 확장 기능은 미션 자동 설정, 코드 실행 및 테스트, 실시간 진행 상황 동기화를 담당한다.

Copilot과의 개발 과정은 요건 정의 (Gemini를 활용한 프롬프트 생성), 데이터베이스 설계 (Prisma 스키마), 단계별 기능 구현 및 디버깅으로 진행되었다. 특히, TypeScript 코드를 브라우저에서 실행하기 위해 정규 표현식을 사용한 간소화된 변환 방식을 Copilot과 논의하여 채택한 점, VS Code 확장 기능과 웹 앱 간의 API 연동을 토큰 기반으로 구현한 점이 특징이다.

Copilot과의 협업을 통해 효과적인 프롬프팅 기술(명확한 컨텍스트 제공, 단계적 상세화, 에러 메시지 공유)을 익혔으며, Copilot이 정형적인 코드 생성이나 에러 수정에 강하고 아키텍처 설계나 복잡한 비즈니스 로직에는 약하다는 점을 파악했다. NextAuth.js v5 정보 부족과 TypeScript 타입 에러 연쇄 발생 등의 난관도 겪었지만, 명확한 버전 지정과 타입 정의를 통해 해결했다.

결과적으로 Copilot을 통해 개발 생산성이 크게 향상되었으며(코드 작성 시간 단축, 프로토타입 개발 가속화), AI가 작성한 코드를 읽고 배우는 사이클을 통해 지속적인 학습을 이어나갈 계획이다.
