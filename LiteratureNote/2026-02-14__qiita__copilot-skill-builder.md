---
type: literature
source_type: web
source_url: https://qiita.com/ricecountry/items/f486f9ebfd1416af3f5c
source_title: "GitHub Copilotで「Copilotの使い方を学ぶアプリ」を作った話"
created_at: "2026-02-14T12:08:37Z"
updated_at: "2026-02-14T12:08:37Z"
tags: [github-copilot, learning, web-app, vscode-extension]
topics: [ai-assisted-development, copilot-education]
---

## TL;DR
- GitHub Copilot 자체를 이용해 "Copilot 사용법을 학습하는 앱" 개발한 메타 학습 사례
- Next.js 14 웹앱 + VS Code 확장 + SQLite(Prisma) 구성
- 단계적 프롬프팅, 컨텍스트 명시, 에러 공유 등 효과적 협업 기법 정리
- TypeScript 브라우저 실행, NextAuth.js v5 인증 등 기술적 난제 해결 과정
- Copilot 득의/불득의 영역 명확히 파악

## 요약 (1350자)
아이스타일 미디어 개발부 와타나베가 작성한 GitHub Copilot 실전 개발 사례이다. 회사에서 Copilot 도입 후 "제대로 활용하고 있는가?"라는 의문에서 출발해, **Copilot 자신에게 Copilot 학습 플랫폼을 만들어달라는 메타 접근**을 시도했다.

**시스템 구성**
- **웹앱(Next.js 14)**: 코스/미션 관리, 진행도 트래킹, NextAuth.js v5 인증, Prisma + SQLite
- **VS Code 확장**: 미션 자동 셋업, 코드 실행/테스트, API 연동으로 실시간 진행도 동기화
- 학습 플로우: 웹앱에서 코스 선택 → 레슨 학습 → VS Code에서 미션 도전 → 코드 작성 → 테스트 실행 → 진행도 자동 반영

**개발 프로세스**
- **요건 정의**: Gemini로 Copilot에 줄 프롬프트 초안 생성
- **DB 설계**: Copilot에게 Prisma 스키마 제안 요청 (User/Course/Lesson/Mission/Progress 모델)
- **구현**: 기본 구조 생성 → 기능별 추가 → 통합 및 디버깅 (에러 메시지를 Copilot에게 전달해 수정)

**기술적 난제 해결**
1. **브라우저에서 TypeScript 실행**: 빌드 도구 없이 정규식 기반 TS→JS 변환으로 학습용 단순화
2. **VS Code 확장 연동**: API 토큰 인증 방식, Bearer 토큰으로 WebAPI 호출
3. **리얼타임 동기화**: `upsert`로 진행도 업데이트, 파일 저장 → 웹앱 반영 → 테스트 실행 → 클리어 판정

**효과적 프롬프팅 기법**
- ❌ 나쁜 예: "인증 기능 만들어줘"
- ✅ 좋은 예: "NextAuth.js v5로 이메일/패스워드 인증, Prisma 어댑터, JWT 세션, bcryptjs 사용, 기존 User 모델 연동"
- **단계적 상세화**: 큰 기능을 작은 스텝으로 분해 (Step 1: 스키마 설계 → Step 2: NextAuth 설정 → ...)
- **에러 공유**: 에러 메시지 그대로 복사해 Copilot에게 전달, import 경로/파일 배치 수정 제안 받음

**Copilot의 득의/불득의**
- 득의: CRUD 등 정형 코드, 기존 패턴 답습, 에러 메시지 수정, 문서 작성
- 불득의: 아키텍처 전체 설계, 트레이드오프 평가, 복잡한 비즈니스 로직, 심층 보안
- 전략: 방향성은 인간이 결정, Copilot에게 구현 맡김

**하마포인트 (난점)**
- NextAuth.js v5 정보 부족: Copilot이 v4 코드 제안 → "v5 (next-auth@beta) 최신 방식으로" 명시 요청
- TypeScript 타입 에러 연쇄: 타입 정의(`types/next-auth.d.ts`)를 먼저 정비 후 구현

**성과**
- 조사→구현 사이클 압도적 단축
- 정형 코드 작성 시간 거의 제로
- 아이디어→프로토타입 시간 대폭 단축
- 메타 학습 효과: Copilot으로 만드는 과정 자체가 최고의 학습

**향후 전망**
- AI가 작성한 코드를 읽고 학습하는 사이클 (코드 리딩 능력 향상)
- `/explain` 등 Copilot 명령으로 AI 코드 해설 활용

저자는 "Copilot은 마법의 지팡이가 아니라 개발자의 의도를 이해하고 빠르게 구현하는 우수한 페어 프로그래머"라고 강조한다.

## 핵심 포인트
- **메타 학습**: Copilot에게 Copilot 학습 앱을 만들게 하는 접근
- **시스템 구성**: Next.js 14 + VS Code 확장 + Prisma/SQLite
- **프롬프팅 원칙**: 컨텍스트 명시 > 단계적 상세화 > 에러 공유
- **기술 난제**: TS→JS 변환(정규식), NextAuth v5, 타입 정의 우선 정비
- **Copilot 역할**: 구현 파트너, 방향성 결정은 인간 몫

## 실무 적용 아이디어
1. **Gemini로 프롬프트 초안 생성**: 막연한 아이디어를 Gemini에 던져 Copilot용 프롬프트로 정제
2. **기술 스택 명시**: "Next.js 14 App Router, TypeScript, Prisma, NextAuth v5" 등 구체적 지정
3. **에러 드리븐 개발**: 에러 메시지를 Copilot에게 그대로 전달해 빠른 수정
4. **타입 정의 우선**: TypeScript 프로젝트는 타입 정의 파일(`*.d.ts`)을 먼저 정비
5. **VS Code 확장 개발**: API 토큰 인증 + WebAPI 연동으로 웹앱과 IDE 통합

## 검증/추가 확인 질문
- 정규식 기반 TS→JS 변환의 한계 시나리오는?
- NextAuth.js v5와 v4의 주요 차이점은?
- GitHub 리포지토리 공개 여부 및 라이선스는?
- VS Code 확장 마켓플레이스 배포 계획은?
- Prisma와 SQLite 조합의 프로덕션 사용 시 고려사항은?

## 메타
- fetched_at: 2026-02-14T12:08:37Z
- author: 渡辺 (アイスタイル メディア開発部)
- published_date: 2025-12-12 (추정)
- platform: Qiita
- github_repo: https://github.com/Watanabe-Keita-wk/copilot-skill-builder
- requirements: Node.js 18.x+, pnpm 8.x+
- article_type: case-study
- failure_log: N/A
