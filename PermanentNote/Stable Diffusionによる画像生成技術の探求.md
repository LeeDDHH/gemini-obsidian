# Stable Diffusionによる画像生成技術の探求

Stable Diffusionは、テキストプロンプトから高品質な画像を生成する強力なAI技術である。しかし、そのポテンシャルを最大限に引き出すには、環境構築からモデルの選定、そしてプロンプトの記述に至るまで、多くの概念を理解する必要がある([[FleetingNote/2026-01-14.md]])。本ノートでは、関連するノート群から得られた知見を統合し、Stable Diffusionを用いた画像生成技術の全体像を整理する。

## I. 基本的な構成要素

Stable Diffusionの画像生成は、主に以下の要素の組み合わせによって制御される。

- **環境**: 画像生成を実行するための基盤。Web UIにはAUTOMATIC1111版が広く使われているが、VRAM使用量を抑え高速化したForge版も存在する([[Mac에 Stable Diffusion Web UI Forge 버전 설치하기.md]])。初心者向けには、必要なモデルや拡張機能が同梱された`EasyReforge`のようなオールインワンパッケージもある([[EasyReforge로 시작하는 무료 로컬 이미지 생성 AI (Stable Diffusion) 2025년판.md]])。
- **モデル (Checkpoint)**: 画像の全体的なスタイル（例：アニメ調、実写風）を決定する基本的な学習データ。`yayoi_mix`のようなリアル系美女に特化したものや、アニメ風の`Hassaku XL`など、多種多様なモデルが存在する([[Stable Diffusion 'yayoi_mix'로 사실적인 미녀를 그려보았다.md]], [[Stable Diffusion에서 Hassaku XL 도입 및 사용 방법.md]])。これらのモデルはCivitAIのようなプラットフォームで入手できる([[Stable Diffusion Web UI 모델(Checkpoint) 사용 방법.md]], [[Stable Diffusion용 CivitAI 사용법.md]])。
- **LoRA (Low-Rank Adaptation)**: モデル全体を再学習することなく、特定のキャラクター、画風、服装、ポーズなどを追加学習させるための軽量なファイル。これにより、基本モデルの能力を拡張し、より狙った通りの画像を生成できる([[Stable Diffusion 'LoRA'란 무엇인가? 추천도 소개.md]], [[Stable Diffusion Web UI LoRA 사용법 (추가 학습 모델).md]])。
- **VAE (Variational Auto Encoder)**: 生成の最終段階で潜像空間からピクセル画像への変換を担い、画像の彩度や色味に影響を与える([[としあき diffusion 위키 - VAE.md]])。

## II. プロンプトエンジニアリングの技術

プロンプトは、AIにどのような画像を生成させたいかを伝えるための具体的な指示である。

- **基本構造**: 生成したい要素を指定する「ポジティブプロンプト」と、除外したい要素を指定する「ネガティブプロンプト」で構成される。特にネガティブプロンプトは、不自然な手足（`extra digit`）などを回避するために極めて重要である([[AI 이미지 생성 네거티브 프롬프트 고찰 (다리 3개, 손가락 6개 등).md]])。
- **Seed値**: 同じプロンプトとSeed値を使えば、同じ画像を再現できる。これを固定することで、キャラクターの一貫性を保ちつつ、服装やポーズのバリエーションを探求することが可能になる([[Stable Diffusion의 시드(Seed) 값 의미와 추천 사용법.md]])。
- **要素の制御**:
    - **顔の表情**: `ahegao`, `blushing`などで感情を表現できる([[Stable Diffusion '에로틱한 얼굴' 프롬프트 (주문).md]])。
    - **体型・服装**: `small breasts`, `kimono`, `maid dress`のように、体型や具体的な服装を細かく指定できる([[AI 이미지 생성 체형 및 가슴 프롬프트 34가지.md]], [[Stable Diffusion 생성 이미지 의상 제어 프롬프트.md]])。
    - **ポーズ**: `arm up`, `m-shaped spread`など、特定のポーズを指示するためのキーワードが存在する([[Stable Diffusion - Illustrious 팔, 손 올리는 동작 프롬프트.md]], [[AI 이미지 생성 M자 개각 프롬프트 가이드.md]])。
- **NSFWコンテンツ**: `nsfw`というキーワードを用いることで、画像の 선정性を制御することが可能。これはポジティブ・ネガティブ双方で利用される([[Stable Diffusion NSFW를 이용한 이미지 선정성 제어 방법.md]], [[Stable Diffusion NSFW 프롬프트 완전 가이드.md]])。

## III. 探求と思考

- プロンプトによる指示は強力だが、複雑な構図や特定のポーズを安定して生成するには限界がある。`ControlNet`のような、より直接的に構図を制御する技術は、この限界をどのように克服するのか？
- `yayoi_mix`の再現実験が示したように、高品質な画像を生成するには`txt2img`だけでなく、`img2img`やアップスケーリングといった後処理が重要になる場合がある([[Stable Diffusion 'yayoi_mix'로 사실적인 미녀를 그려보았다.md]])。高品質な作品を生み出すための、体系的なワークフローとはどのようなものか？
- モデルやLoRAにはそれぞれライセンスが存在する。生成した画像の商用利用や公開に関して、どのような倫理的配慮と実践が必要か？

## IV. 関連ノート

- [[FleetingNote/2026-01-14.md]]
- [[LiteratureNote/EasyReforge로 시작하는 무료 로컬 이미지 생성 AI (Stable Diffusion) 2025년판.md]]
- [[LiteratureNote/Mac에 Stable Diffusion Web UI Forge 버전 설치하기.md]]
- [[LiteratureNote/Stable Diffusion 'LoRA'란 무엇인가? 추천도 소개.md]]
- [[LiteratureNote/Stable Diffusion Web UI LoRA 사용법 (추가 학습 모델).md]]
- [[LiteratureNote/Stable Diffusion Web UI 모델(Checkpoint) 사용 방법.md]]
- [[LiteratureNote/Stable Diffusion 'yayoi_mix'로 사실적인 미녀를 그려보았다.md]]
- [[LiteratureNote/Stable Diffusion - Illustrious 팔, 손 올리는 동작 프롬프트.md]]
- [[LiteratureNote/Stable Diffusion NSFW 프롬프트 완전 가이드.md]]
- [[LiteratureNote/Stable Diffusion NSFW를 이용한 이미지 선정성 제어 방법.md]]
- [[LiteratureNote/Stable Diffusion 생성 이미지 의상 제어 프롬프트.md]]
- [[LiteratureNote/Stable Diffusion 에로틱한 얼굴 프롬프트 (주문).md]]
- [[LiteratureNote/Stable Diffusion에서 Hassaku XL 도입 및 사용 방법.md]]
- [[LiteratureNote/Stable Diffusion의 시드(Seed) 값 의미와 추천 사용법.md]]
- [[LiteratureNote/Stable Diffusion용 CivitAI 사용법.md]]
- [[LiteratureNote/AI 이미지 생성 M자 개각 프롬프트 가이드.md]]
- [[LiteratureNote/AI 이미지 생성 네거티브 프롬프트 고찰 (다리 3개, 손가락 6개 등).md]]
- [[LiteratureNote/AI 이미지 생성 체형 및 가슴 프롬프트 34가지.md]]
- [[LiteratureNote/としあき diffusion 위키 - VAE.md]]
