---
type: permanent
theme: "AI 주도 개발 워크플로우: Claude Code와 Copilot CLI의 실전 적용"
created_at: "2026-02-14T21:31:31+09:00"
updated_at: "2026-02-14T21:31:31+09:00"
tags: [ai-development, claude-code, copilot-cli, workflow, automation]
topics: [developer-tools, ai-agents, code-review]
source_notes:
  - "[[Claude Code와 CodeRabbit을 활용한 AI 개발 워크플로우]]"
  - "[[2026-02-14__zenn__copilot-cli]]"
  - "[[외국계 IT 엔지니어의 AI 활용법 (Claude Code, CodeRabbit)]]"
  - "[[2026-02-13__youtube.com__바이브코딩으로-1시간-만에-AI-서비스-런칭하기]]"
  - "[[Claude Code로 블로그 작성 효율화하는 방법]]"
  - "[[Claude Code를 처음 사용하는 사람들을 위한 실용 가이드]]"
---

# AI 주도 개발 워크플로우: Claude Code와 Copilot CLI의 실전 적용

## TL;DR
- AI 코딩 에이전트가 코딩 작업 자체를 병목에서 해방시켰고, 이제는 **코드 리뷰가 새로운 병목**이 됨
- Claude Code, GitHub Copilot CLI는 터미널 기반 AI 에이전트로 프롬프트 기반 개발과 MCP(Model Context Protocol) 연동을 지원
- 외국계 IT 엔지니어들은 Claude Code + CodeRabbit 조합으로 코딩과 리뷰 자동화를 동시에 달성
- "코드 90%를 AI가 작성하는 세계"가 현실이 되면서 개발자의 역할이 **의사결정, 설계, 품질 관리**로 이동 중

## 내 결론(현재 시점)
**AI 개발 도구는 단순 자동완성을 넘어 "에이전트"로 진화했다.** 코딩 속도 향상보다 중요한 것은 **워크플로우 재설계**다. 개발자는 이제 "코드 작성자"가 아닌 "AI 프롬프터 + 의사결정자 + 리뷰어"로 역할 전환이 필요하다.

### 핵심 패러다임 전환
1. **병목의 이동**: 코딩 → 코드 리뷰 → 의사결정/설계
2. **도구의 진화**: 자동완성 → 대화형 에이전트 → MCP 기반 확장 생태계
3. **개발 프로세스**: 수동 코딩 → 프롬프트 기반 코드 생성 → AI 리뷰(CodeRabbit) → 인간 최종 검증

### 실무에서 검증된 효과
- **바이브코딩**: 1시간 만에 AI 서비스 프로토타입 완성
- **복수 리포지토리 작업**: Copilot CLI로 system-A, system-B 통합 스크립트 자동 생성 (~10분)
- **블로그 작성**: Claude Code로 초안 → 편집 → 배포 워크플로우 자동화
- **코드 리뷰 병목 해소**: CodeRabbit이 PR 자동 리뷰 (OSS 무료)

## 근거(참조 링크)

### Claude Code 실전 활용
- [[Claude Code와 CodeRabbit을 활용한 AI 개발 워크플로우]]
  - 외국계 IT 엔지니어의 라이브 코딩 데모
  - "코딩이 더 이상 병목이 아니고, 리뷰어가 병목이다"
- [[외국계 IT 엔지니어의 AI 활용법 (Claude Code, CodeRabbit)]]
  - Claude Code: 프롬프트 기반 코드 생성
  - CodeRabbit: 오픈소스 프로젝트 무료, $100만 기부로 커뮤니티 지원
- [[Claude Code로 블로그 작성 효율화하는 방법]]
  - 비개발 작업(글쓰기, 문서 작성)에도 확장 가능

### GitHub Copilot CLI 기술 상세
- [[2026-02-14__zenn__copilot-cli]]
  - 설치: `npm install -g @github/copilot`
  - 기본 모델: Claude Sonnet 4.5 (GPT-5, Claude Sonnet 4 선택 가능)
  - MCP Server 연동: GitHub MCP 기본 탑재, 추가 MCP 등록 가능
  - 도구 권한 제어: `--allow-tool 'write'`, `--deny-tool`
  - 세션 관리: `--continue` (직전 세션), `--resume` (선택 세션)
  - 커스텀 인스트럭션: `.github/`, `CLAUDE.md`, `GEMINI.md` 자동 참조
  - 실전 사례: 복수 GitHub 리포(system-A, system-B) 대상 DynamoDB + PostgreSQL 셸 스크립트 자동 생성 (~10분)

### 바이브코딩과 AI 자율 개발
- [[2026-02-13__youtube.com__바이브코딩으로-1시간-만에-AI-서비스-런칭하기]]
  - 프롬프트만으로 1시간 내 AI 서비스 프로토타입 완성
  - "느낌과 분위기"로 개발하는 새로운 패러다임

### 온보딩 가이드
- [[Claude Code를 처음 사용하는 사람들을 위한 실용 가이드]]
  - 초기 설정, 신뢰 디렉토리, MCP 연동 단계별 가이드

## 반례/제약/리스크

### 1. 코드 품질 저하 가능성
- AI 생성 코드의 품질이 항상 높지는 않음
- 특히 복잡한 비즈니스 로직, 엣지 케이스 처리 미흡
- **대응**: 인간 리뷰어가 설계 의도와 일치하는지 최종 검증 필수

### 2. 리뷰 병목의 한계
- CodeRabbit 등 AI 리뷰 도구도 만능은 아님
- 아키텍처 결정, 트레이드오프 판단은 인간 영역
- **대응**: 리뷰 자동화 + 인간의 전략적 리뷰 분리

### 3. 과도한 의존 리스크
- AI 도구 없이는 코딩 불가능한 개발자 양산 우려
- 기본기(알고리즘, 자료구조, 디버깅) 약화
- **대응**: AI는 도구일 뿐, 기본기는 여전히 필수

### 4. 비용 문제
- GitHub Copilot CLI: 유료 플랜(Pro/Pro+/Business/Enterprise) 필요
- 프롬프트당 1 프리미엄 리퀘스트 소비
- **대응**: 개인 프로젝트는 무료 대안(Claude Code) 활용

### 5. 보안/신뢰성
- 신뢰 디렉토리 설정 누락 시 보안 위험
- MCP Server 추가 시 악의적 도구 위험
- **대응**: 공식 MCP만 사용, `--deny-tool`로 위험 도구 차단

## 실무 적용 체크리스트

### 초기 설정 (1일)
- [ ] Claude Code 또는 Copilot CLI 설치
- [ ] 신뢰 디렉토리 설정 (홈 디렉토리 실행 금지)
- [ ] 커스텀 인스트럭션 작성 (`.github/copilot-instructions.md`, `CLAUDE.md`)
- [ ] MCP Server 연동 (필요 시)

### 워크플로우 최적화 (1주)
- [ ] 프롬프트 패턴 정립 (파일 컨텍스트 지정, 작업 분리)
- [ ] 도구 권한 프리셋 alias 등록 (`--allow-tool 'write'` 등)
- [ ] 세션 관리 규칙 설정 (`--continue` vs `--resume`)
- [ ] 리뷰 자동화 도구 도입 (CodeRabbit, PR 템플릿)

### 팀 적용 (1개월)
- [ ] 팀 공통 인스트럭션 작성 (코드 스타일, 네이밍 규칙)
- [ ] AI 생성 코드 리뷰 체크리스트 작성
- [ ] 바이브코딩 vs 정밀 설계 적용 기준 수립
- [ ] KPI 추적: 개발 속도, 버그율, 리뷰 시간

### 지속적 개선 (3개월~)
- [ ] AI 도구 없이 코딩하는 날 주 1회 유지 (기본기 유지)
- [ ] 새 MCP Server 탐색 (Playwright, Chrome DevTools 등)
- [ ] 프롬프트 엔지니어링 노하우 문서화
- [ ] 팀 내 AI 활용 사례 공유 세션

## 다음 질문/다음 행동(PoC/실험)

### 검증 필요 질문
1. **Copilot CLI vs Claude Code 정량 비교**  
   - 속도, 정확도, 비용, MCP 생태계 비교 실험 필요
2. **AI 생성 코드의 버그율 측정**  
   - AI 작성 vs 인간 작성 코드의 실제 결함 밀도 비교
3. **리뷰 병목 해소 효과 측정**  
   - CodeRabbit 도입 전후 PR 머지 시간, 리뷰 코멘트 수 비교
4. **MCP Server 보안 가이드라인**  
   - 추가 MCP의 보안 검증 체크리스트 필요
5. **GitHub CLI 확장 copilot 폐지 일정**  
   - 마이그레이션 계획 수립 필요

### 다음 실험 (PoC)
- [ ] **실험 1**: 신규 기능 개발 시 Copilot CLI만으로 1주 개발
  - 목표: 순수 프롬프트 기반 개발 가능성 검증
- [ ] **실험 2**: CodeRabbit 무료 OSS 프로젝트에 도입
  - 목표: 리뷰 자동화 효과 측정
- [ ] **실험 3**: 팀 공통 `.github/copilot-instructions.md` 작성
  - 목표: 일관된 코드 스타일 자동 유지
- [ ] **실험 4**: MCP Server로 Playwright 연동
  - 목표: E2E 테스트 자동 생성 가능성 확인

### 추가 탐색 영역
- **Cursor vs Claude Code vs Copilot CLI** 비교
- **AI Pair Programming 패턴** 문서화
- **프롬프트 엔지니어링 베스트 프랙티스** 축적
- **AI 생성 코드 테스트 커버리지** 자동 검증

---

## 관련 PermanentNote
- [[모던 프론트엔드 개발 (React & Next.js)]] - AI 도구와 프런트엔드 개발의 결합
- [[AI를 활용한 정보 수집 및 지식 정리 효율화]] - 개발 외 AI 활용 확장
- [[소프트웨어 공학 철학 및 팀 역학]] - AI 시대의 리뷰 문화 재정립
