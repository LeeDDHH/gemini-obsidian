# Stable Diffusion을 활용한 AI 이미지 생성 탐구

## 요약

이 노트는 Stable Diffusion을 사용하여 고품질 AI 이미지를 생성하는 다양한 기술과 프롬프트 엔지니어링 전략을 탐구합니다. 프롬프트 작성의 기본 원칙부터 시작하여, `img2img`와 같은 고급 기능, 모델(Checkpoint) 및 LoRA 활용법, 그리고 이미지의 특정 요소(의상, 신체 부위, NSFW 등)를 정밀하게 제어하는 방법에 이르기까지 광범위한 주제를 다룹니다. 또한, 생성된 이미지의 품질을 높이고 기형을 방지하기 위한 실용적인 팁과 도구(Tagger, Prompt Peek)도 함께 살펴봅니다.

## 관련된 메모

- [[Stable Diffusion 이미지에서 프롬프트 추정 방법]]
- [[Stable Diffusion img2img 활용법]]
- [[Stable Diffusion 유두 고문 재현 프롬프트]]
- [[Stable Diffusion 사정 및 정액 재현 프롬프트 가이드]]
- [[Stable Diffusion 옷 상태 재현 프롬프트]]
- [[Stable Diffusion img2img 사용법]]
- [[AI 일러스트 프롬프트 확인 도구 Prompt Peek]]
- [[Stable Diffusion 괄호 강조 구문 활용법]]
- [[Stable Diffusion 이미지 기형 방지 방법]]
- [[Stable Diffusion 여자친구 생성 프롬프트 100선]]
- [[AI 일러스트 프롬프트 작성 황금 템플릿]]
- [[Stable Diffusion 'yayoi_mix'로 사실적인 미녀를 그려보았다]]
- [[Stable Diffusion의 시드(Seed) 값 의미와 추천 사용법]]
- [[EasyReforge로 시작하는 무료 로컬 이미지 생성 AI (Stable Diffusion) 2025년판]]
- [[Stable Diffusion 모델 XeroxRealMix 사용 방법]]
- [[Stable Diffusion에서 Hassaku XL 도입 및 사용 방법]]
- [[Stable Diffusion WEBUI 생성 환경 정보]]
- [[AI 이미지 생성 체형 및 가슴 프롬프트 34가지]]
- [[Stable Diffusion용 CivitAI 사용법]]
- [[Stable Diffusion Web UI LoRA 사용법 (추가 학습 모델)]]
- [[Stable Diffusion LoRA란 무엇인가 추천도 소개]]
- [[NovelAI Diffusion 위키 - 생성 주문 (프롬프트)]]
- [[Stable Diffusion 생성 이미지 의상 제어 프롬프트]]
- [[Stable Diffusion NSFW를 이용한 이미지 선정성 제어 방법]]
- [[Stable Diffusion NSFW 필터 우회 비활성화 방법]]
- [[Stable Diffusion 에로틱한 얼굴 프롬프트 (주문)]]
- [[AI 이미지 생성 네거티브 프롬프트 고찰 (다리 3개, 손가락 6개 등)]]
- [[Stable Diffusion 모유 및 착유기 재현 프롬프트]]
- [[Stable Diffusion - Illustrious 팔, 손 올리는 동작 프롬프트]]
- [[AI 이미지 생성 M자 개각 프롬프트 가이드]]
- [[Stable Diffusion NSFW 프롬프트 완전 가이드]]
- [[Stable Diffusion yayoi_mix로 사실적인 미녀를 그려보았다]]
- [[Stable Diffusion Web UI 모델(Checkpoint) 사용 방법]]
- [[Stable Diffusion에서 NSFW(성인) 이미지 생성 가능 여부 검증]]
- [[Mac에 Stable Diffusion Web UI Forge 버전 설치하기]]
- [[Mac에 Stable Diffusion Web UI 설치하기]]
- [[【Obsidian】KindleのハイライトをAIに要約してもらう]]
- [[Kindle 책을 빠르게 텍스트로 변환하여 AI 도구로 활용하는 방법(Mac 한정)]]
- [[Kindle本を高速でテキスト化し、AIツールで活用する方法（Mac限定）]]
- [[コードの90パーセントをAIが書く世界で何が待っているのか.ko]]
- [[コードの90パーセントをAIが書く世界で何が待っているのか]]

## 발전적인 질문

- 현재의 프롬프트 엔지니어링 기술을 넘어, 이미지 생성의 질과 제어 가능성을 더욱 높일 수 있는 새로운 접근법은 무엇일까요?
- 다양한 모델과 LoRA의 조합이 만들어내는 시너지는 어떻게 체계적으로 탐색하고 정리할 수 있을까요?
- AI 이미지 생성 기술의 발전이 창의성, 저작권, 그리고 윤리적 문제에 미치는 영향은 무엇이며, 우리는 어떻게 대응해야 할까요?
- NSFW 콘텐츠 생성과 관련하여, 기술적 제어와 표현의 자유 사이의 균형을 어떻게 맞출 수 있을까요?