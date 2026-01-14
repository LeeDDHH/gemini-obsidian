# Stable Diffusion 이미지에서 프롬프트 추정 방법

## 요약
이 기사는 Stable Diffusion에서 이미지로부터 프롬프트를 추정하는 세 가지 방법(Interrogate CLIP, Interrogate DeepBooru, Tagger)을 소개한다. Interrogate CLIP은 연결된 문장 형태의 프롬프트를 생성하며, DeepBooru는 애니메이션/일러스트 이미지에 특화되어 쉼표로 구분된 단어 프롬프트를 생성한다. Tagger는 확장 기능으로 설치해야 하며, 역시 쉼표로 구분된 단어 프롬프트를 제공한다. 이 기능들을 통해 효율적인 이미지 생성이 가능하며, GPU 클라우드 서비스인 GPUSOROBAN을 활용하면 더욱 자유로운 이미지 생성을 할 수 있다고 설명한다.

## 원본 URL
https://soroban.highreso.jp/article/article-084