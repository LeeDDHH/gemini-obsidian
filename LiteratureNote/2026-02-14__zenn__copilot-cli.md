---
type: literature
source_type: web
source_url: https://zenn.dev/10q89s/articles/4a42fb779fde89
source_title: "GitHub Copilot CLI を使ってみた"
created_at: "2026-02-14T12:08:37Z"
updated_at: "2026-02-14T12:08:37Z"
tags: [github-copilot, cli, ai-tools, development]
topics: [developer-tools, copilot-cli]
---

## TL;DR
- GitHub Copilot CLI (2025/9/25 public preview) 소개 및 사용 가이드
- npm으로 설치 가능, 인터랙티브 모드로 대화형 코딩 지원
- 설정 파일, MCP Server 연동, 신뢰 디렉토리 관리 등 상세 설명
- 복수 리포지토리 대상 스크립트 생성 실전 사례 제시
- Claude Sonnet 4.5, GPT-5 등 모델 선택 가능

## 요약 (1200자)
URBAN HACKS의 이케다 엔지니어가 작성한 GitHub Copilot CLI 실전 가이드이다. 2025년 9월 25일 public preview로 공개된 Copilot CLI는 유료 플랜(Pro/Pro+/Business/Enterprise)에서 사용 가능하며, 프롬프트당 1 프리미엄 리퀘스트를 소비한다. 기존 GitHub CLI 확장판 copilot은 폐지 예정이다.

**설치 및 기본 사용**
- `npm install -g @github/copilot`로 설치, `copilot` 명령으로 인터랙티브 모드 시작
- 초회 실행 시 귀여운 Copilot 애니메이션 표시 (이후 `--banner` 플래그로 재생 가능)
- 신뢰 디렉토리 확인: 홈 디렉토리에서 실행 비권장, 신뢰할 수 있는 프로젝트 디렉토리에서만 사용

**주요 설정**
- 설정 파일: `$HOME/.copilot/config.json`
- 기본 모델: Claude Sonnet 4.5 (GPT-5, Claude Sonnet 4 선택 가능)
- `--model` 플래그 또는 `/model` 명령으로 모델 변경 가능
- `COPILOT_MODEL` 환경 변수로 기본 모델 지정

**MCP Server 연동**
- 기본적으로 GitHub MCP Server 탑재
- `/mcp add <server-name>` 명령으로 추가 MCP Server 등록
- Context7 등 외부 MCP Server 연동 가능
- 설정은 `$HOME/.copilot/mcp-config.json`에 저장

**커스텀 인스트럭션**
- `.github/instructions/**/*.instructions.md`
- `.github/copilot-instructions.md`
- `CLAUDE.md`, `GEMINI.md`, `$HOME/.copilot/copilot-instructions.md` 등 자동 참조
- 기존 GitHub Copilot 인스트럭션과 호환

**코드 생성 Tips**
- `@<file-path>`로 컨텍스트 파일 지정 (복수 지정 가능)
- 세션 관리: `--continue`로 직전 세션 재개, `--resume`로 이전 세션 선택
- 도구 자동 허가: `--allow-tool 'write'`로 파일 편집 권한 부여, `--deny-tool`로 특정 도구 거부
- `-p` 또는 `--prompt` 플래그로 non-interactive 모드 실행 가능

**실전 사례**
- 복수 GitHub 리포지토리(system-A, system-B)를 대상으로 로컬 데이터 셋업 스크립트 생성
- DynamoDB와 PostgreSQL 연동 shell script 자동 생성 (~10분 소요)
- VS Code의 GitHub Copilot Agent 모드와 유사한 결과 생성

장점: 터미널에서 Copilot 활용 가능, MCP 연동으로 확장성, 기존 인스트럭션 재사용
단점: Claude Code 등 타 터미널 AI 에이전트 대비 일부 기능 부족 (하지만 빠르게 업데이트 중)

## 핵심 포인트
- **설치**: `npm install -g @github/copilot`, `copilot` 실행
- **모델**: Claude Sonnet 4.5 (기본), GPT-5, Claude Sonnet 4 선택 가능
- **MCP Server**: GitHub MCP 기본 탑재, 추가 MCP Server 연동 가능
- **도구 권한**: `--allow-tool 'write'`, `--deny-tool` 플래그로 세밀한 제어
- **세션 관리**: `--continue` (직전 세션), `--resume` (선택 세션)
- **인스트럭션**: `.github/`, `CLAUDE.md`, `GEMINI.md` 등 자동 참조

## 실무 적용 아이디어
1. **셸 스크립트 자동화**: 복잡한 셸 스크립트를 프롬프트로 요청해 자동 생성
2. **복수 리포지토리 작업**: 여러 프로젝트 디렉토리를 컨텍스트로 포함해 통합 스크립트 생성
3. **MCP 연동 활용**: Playwright MCP, Chrome DevTools MCP 등과 연동해 브라우저 자동화
4. **인스트럭션 표준화**: 팀 공통 `.github/copilot-instructions.md` 작성으로 일관된 코드 스타일 유지
5. **도구 허가 프리셋**: `--allow-tool 'write' --allow-tool 'shell(git)'` 등을 alias로 등록해 재사용

## 검증/추가 확인 질문
- GitHub CLI 확장 copilot의 폐지 일정은 언제인가?
- 프리미엄 리퀘스트의 요금 체계는?
- MCP Server 추가 시 보안 고려사항은?
- Claude Code 대비 Copilot CLI의 강점/약점 비교는?
- 신뢰 디렉토리 설정을 프로젝트별로 자동화하는 방법은?

## 메타
- fetched_at: 2026-02-14T12:08:37Z
- author: 池田 (URBAN HACKS)
- published_date: 2025-09-26 (추정)
- platform: Zenn.dev
- article_type: tutorial
- failure_log: N/A
