# How to Build a Next.js App + Prisma with Ai in ONE PROMPT
https://www.youtube.com/watch?v=Aqkc95jtHzM

## **요약 (한국어)**

- 영상은 **Prisma 개발자(Arthur)**가 “**단 하나의 프롬프트**”로 **Next.js + Postgres + Prisma**가 연결된 **완성형 템플릿 앱**을 라이브로 부트스트랩하는 흐름을 보여줍니다. 스키마, 마이그레이션, 시드(seed) 데이터, API, 페이지까지 한 번에 생성해 “다음 SaaS/포트폴리오/부트스트랩”에 바로 쓰게 만드는 게 목표입니다.
    
- 진행 방식은 create-next-app으로 새 Next.js 프로젝트를 만든 뒤, 루트에 **prompt.md**를 만들고 Prisma 문서의 “Next + Prisma + AI” 가이드에 있는 프롬프트를 그대로 붙여 넣습니다. 이후 IDE(CURSOR 등)에서 AI 패널을 열어 **prompt.md****를 참조해서 가이드라인대로 실행/구현하라**고 지시하고, 에이전트 모드로 작업을 맡깁니다.
    
- AI가 자동으로 **Prisma 관련 패키지 설치**, **prisma/** **폴더 생성**, **Prisma 설정 파일/스키마 파일 작성**, **seed 스크립트 추가**, **page.tsx****에서 DB에서 유저를 조회해 표시**, **API 라우트(root.ts 등)에서 GET/POST 핸들러 구성** 등을 단계별 TODO 리스트로 진행합니다.
    
- 이후 로컬 실행 검증을 위해 터미널에서 **prisma generate**를 실행하고, package.json에 이미 구성된 스크립트로 **Prisma Studio(npm run db:studio 같은 형태)**를 열어 시드로 들어간 **demo 유저 데이터**가 실제로 생성됐는지 확인합니다. 마지막으로 **npm run dev**로 3000 포트에서 앱을 띄워 페이지가 DB의 유저 데이터를 정상적으로 불러오는지 확인하고 마무리합니다.
    
- 핵심 메시지는 “**프롬프트 파일을 레퍼런스로 고정**하고, 에이전트에게 설치/스키마/마이그레이션/시드/코드 수정/실행 검증까지 한 번에 시켜서 **초기 세팅 시간을 크게 줄이자**” 입니다.
    

  

## **키워드 (최대 3개)**

- **Next.js + Prisma**
    
- **PostgreSQL 연동**
    
- **One-shot Prompt / Agent Mode**