# Stable Diffusion API 개발 조사 메모

**Original URL**: https://zenn.dev/silverbirder/scraps/3842c715662551

**Summary**:
Stable Diffusion API 개발에 대한 조사 노트는 Stable Diffusion을 API로 만들어 Slack 봇 등과 연동하는 것을 목표로 합니다. Stable Diffusion은 이미지 생성에 GPU가 필요하며, 최소 10GB VRAM을 가진 GPU에서 실행됩니다. API화를 위한 시스템 설계안으로 Google Colaboratory, GCE with GPU, Cloud Run for Anthos, Cloud Batch, stability.ai API 사용 등을 고려했습니다. 비용 효율성 때문에 stability.ai API 사용을 최종 선택했으며, 개인적인 사용에는 stability-sdk가 적합하다는 결론을 내렸습니다. 또한, GPU 없이 Stable Diffusion을 실행할 수 있는 `bes-dev/stable_diffusion.openvino`도 언급되었습니다.

**Date Created**: 2026-01-15