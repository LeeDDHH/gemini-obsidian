# としあき diffusion 위키 - VAE

**원본 URL**: https://wikiwiki.jp/sd_toshiaki/VAE

**요약**:
VAE는 이미지 생성 AI인 Stable Diffusion에서 중요한 역할을 하는 Variational Auto Encoder의 약자이다. 이는 잠재 공간(latent space)과 일반적인 이미지 형식 사이를 연결하며, 이미지 생성의 마지막 단계에서 잠재 공간 표현을 이미지로 디코딩하거나, img2img에서는 이미지를 잠재 공간 표현으로 인코딩한다. VAE는 이미지의 채도나 색감에 영향을 주며, 적절한 VAE를 사용하지 않으면 이미지가 회색빛이 되거나 보라색 반점이 생길 수 있다. Stable Diffusion은 VAE를 활용하여 계산 비용을 절감하고 개인 PC에서도 이미지 생성이 가능하게 만들었다.
