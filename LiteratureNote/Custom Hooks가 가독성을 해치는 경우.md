# Custom Hooks가 가독성을 해치는 경우

**Original URL**: https://zenn.dev/yuta_takahashi/articles/cases-where-custom-hooks-compromise-readability

**Summary**:
React 커스텀 훅은 재사용성이 없는 로직을 분리하거나 라이브러리 훅을 래핑할 때 가독성을 해칠 수 있습니다. 과도한 추상화는 응집도를 낮추고, 라이브러리 훅을 감싸는 것은 불안정한 인터페이스를 만들 수 있습니다. 커스텀 훅은 도메인에 독립적인 범용적인 책임, 단일 책임의 부작용 캡슐화, 라이브러리 훅의 명확한 정책화, 부모-자식 간 상태 리프트업 계약이 있을 때 고려해야 합니다. 핵심은 성급한 추상화를 피하고 응집도와 투명성을 유지하는 것입니다.

**Date Created**: 2026-01-15