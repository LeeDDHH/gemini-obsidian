# Build & Deploy an AI Chat App: Next.js App Router Tutorial
https://www.youtube.com/watch?v=R06lJ3RlxIg

## **요약 (Next.js + Cohere + Vercel로 스트리밍 AI 채팅앱 만들고 배포)**

  

이 내용은 **Next.js(App Router)**로 간단한 AI 채팅(질문 → 답변) 웹앱을 만들고, **Cohere 텍스트 생성 API**로 응답을 **스트리밍** 형태로 받아 UI에 보여준 뒤, 최종적으로 **Vercel**에 배포하는 전체 흐름을 설명합니다.

  

핵심 진행 순서는 다음과 같습니다.

1. **Next.js 프로젝트 생성**
    
    - npx create-next-app@latest로 프로젝트를 새로 만들고
        
    - TypeScript/ESLint는 사용하지 않으며, **Tailwind CSS**와 **App Router**를 선택합니다.
        
    
2. **환경변수 설정**
    
    - 루트 디렉토리에 .env.local 파일을 만들고 **Cohere API Key**를 저장합니다.
        
    - Cohere 대시보드( API Keys )에서 무료 트라이얼 키를 발급받아 사용합니다.
        
    - 예: COHERE_API_KEY=...
        
    
3. **API Route(백엔드) 구현: Cohere 스트리밍 연결**
    
    - app/api/completion/route.js 같은 형태로 라우트 핸들러를 만들고,
        
    - Vercel의 ai 라이브러리에서 제공하는 스트리밍 유틸을 사용합니다.
        
    - 이 라우트는 **Edge Runtime**으로 동작하도록 설정합니다.
        
    - 사용자가 입력한 prompt를 받아 Cohere의 generate 엔드포인트로 POST 요청을 보내고,
        
    - 성공 시 응답을 스트리밍으로 변환해 StreamingTextResponse로 클라이언트에 전달합니다.
        
    - 중간에 발생한 실수로 prompt가 undefined가 되었는데, 원인은 **request body를 JSON으로 파싱하지 않아서**였고, await req.json()으로 해결합니다.
        
    
4. **프론트엔드 UI 구현: useCompletion Hook**
    
    - app/page.js를 use client 컴포넌트로 만들고,
        
    - ai/react의 useCompletion 훅을 사용해 입력/전송/로딩/중단(stop)/결과(completion)를 연결합니다.
        
    - 폼 제출 시 /api/completion로 요청을 보내고, 응답은 스트리밍으로 화면에 출력됩니다.
        
    - “Stop” 버튼으로 스트리밍 생성 중단도 가능하게 구성합니다.
        
    
5. **Tailwind로 스타일링**
    
    - 기본 UI가 투박하니, Tailwind 클래스를 적용해 **미래지향적인 챗 UI**처럼 꾸밉니다.
        
    - 전체 화면 채우기(h-screen)와 여백/패딩 조정 등으로 보기 좋게 정리합니다.
        
    
6. **GitHub에 푸시**
    
    - 새 레포지토리를 만들고 로컬 프로젝트를 git add/commit/push로 업로드합니다.
        
    
7. **Vercel 배포**
    
    - Vercel에서 GitHub 레포를 Import하여 프로젝트 생성
        
    - 배포 전 **Environment Variable**로 COHERE_API_KEY를 등록
        
    - Deploy 후 제공되는 URL에서 동일하게 스트리밍 응답이 동작하는지 확인합니다.
        
    

---

## **주의/트러블슈팅 포인트**

- **req에서 prompt가 undefined일 때**
    
    - 프론트에서 JSON으로 보내면 서버에서 반드시 await req.json()으로 파싱해야 합니다.
        
    
- **Edge Runtime 설정**
    
    - 스트리밍 유틸/런타임 요구사항 때문에 export const runtime = "edge" 설정이 필요합니다.
        
    
- **Vercel 배포 시 환경변수 등록 필수**
    
    - 로컬의 .env.local은 배포 환경에 자동 반영되지 않으니 Vercel 대시보드에 반드시 추가해야 합니다.
        
    

---

## **키워드 (최대 3개)**

- **Next.js App Router**
    
- **Cohere API (Streaming)**
    
- **Vercel Deploy**