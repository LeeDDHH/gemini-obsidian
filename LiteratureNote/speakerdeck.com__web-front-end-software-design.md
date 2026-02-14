---
type: literature
source_url: https://speakerdeck.com/tooppoo/consideration-of-software-design-in-web-front-end?slide=2
source_title: WEBフロントエンドにおけるソフトウェア設計の考察 / Consideration of software design in WEB front end
created_at: 2026-02-14
updated_at: 2026-02-14
tags:
  - frontend
  - software-design
  - architecture
source_type: speakerdeck
author_name: philomagi
author_url: https://speakerdeck.com/tooppoo
has_slide_text: true
has_pdf: false
slide_count: 132
---

현대 웹 프론트엔드는 단순한 화면 표시를 넘어 로직과 도메인을 포함하며, 화면 설계뿐 아니라 소프트웨어 설계가 필수적이다. 사용자 조작과 화면 상태의 복잡성 증가는 프론트엔드의 난이도를 높이며, 이를 효과적으로 관리하는 것이 중요하다.

프론트엔드에도 '도메인'이 존재한다. 이는 사용자의 활동과 관심사에 해당하며, 백엔드가 데이터 일관성으로 도메인을 해결한다면, 프론트엔드는 표시, 조작, 상태 관점에서 도메인 문제를 해결한다. 핵심은 사용자가 '했다'고 생각한 것을 화면에 구현하는 것이다.

프론트엔드 설계는 UI 디자인 같은 '화면 설계'와 모듈 설계 같은 '소프트웨어 설계' 두 가지가 있다. Atomic Design은 UI 컴포넌트 조립에 유용하지만 소프트웨어 설계 전체를 다루지는 못한다. 객체 지향은 조작과 표시에 관련된 요소를 객체로 추출하여 프론트엔드 소프트웨어 설계에 효과적인 방법론이다.

아키텍처 측면에서 Flux는 단방향 데이터 흐름에 강력하지만, 모듈 구조를 정의하지 않아 다른 아키텍처와 결합해야 한다. 클린 아키텍처는 프론트엔드에서도 유효하며, 핵심은 의존성 규칙(내부 지향)을 지키는 것이다. UI 계층 내에서도 클린 아키텍처의 동심원 구조를 적용하여 계층을 정리할 수 있다.

결론적으로, 프론트엔드 개발자는 도메인과 설계를 분석하고 이해하며 자부심을 가지고 임해야 한다. 향후 사용자 환상, OOUI, 프론트엔드 DDD, CQRS(+ES)로서의 Flux 재분석 등이 중요한 과제가 될 것이다.
