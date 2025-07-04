이 기사는 Nifty 주식회사의 와타나베 다이스케 씨가 자사 홈페이지 개발에서 얻은 지식을 바탕으로 Next.js를 사용한 웹사이트의 표시 속도를 개선하는 구체적인 방법을 소개한 것입니다.

먼저, 페이지 로딩 부하의 큰 요인인 이미지 파일에 대한 대책으로 Next.js의 `next/image` 컴포넌트 활용을 들고 있습니다. 일반적으로 웹 개발에서는 이미지 크기를 압축하거나 가벼운 형식으로 변환하는 수고가 발생하지만, `next/image`는 지정된 이미지를 Google이 개발한 차세대 이미지 형식인 WebP로 자동 변환합니다. 기사 내의 비교 예시에서는 일반적인 `<img>` 태그로 표시했을 때와 비교하여 화질 저하를 느끼게 하지 않으면서 파일 크기를 1/10 이하로 줄일 수 있어 그 효과의 높이를 보여주고 있습니다.

다음으로, JavaScript 로딩 양을 줄이는 방법으로 `next/dynamic`(동적 임포트)을 소개하고 있습니다. 이것은 토글 메뉴나 모달 창처럼 페이지 최초 표시 시에는 필요 없는 컴포넌트를 사용자가 실제로 그 기능을 조작한 시점에 로드하도록 하는 구조입니다. 이를 통해 초기 로드 데이터 양이 줄어들어 표시 시작까지의 시간을 단축할 수 있습니다.

또한, 성능에 큰 영향을 미치기 쉬운 외부 스크립트 로딩에 대해서는 `next/script` 컴포넌트가 유효하다고 말합니다. 이 컴포넌트의 `strategy` 속성(`beforeInteractive`, `afterInteractive`, `lazyOnload`)을 구분하여 사용함으로써 스크립트 로딩 타이밍을 제어할 수 있습니다. 예를 들어, 광고 등 바로 실행하고 싶은 스크립트는 최우선으로, 챗봇처럼 페이지 표시가 완료된 후에 로드해도 되는 것은 나중에 로드하는 등 우선순위를 자동으로 정하여 렌더링을 최적화합니다.

마지막으로, 성능 개선의 병목 현상을 특정하기 위해 '번들 크기 가시화'의 중요성을 강조하고 있습니다. `@next/bundle-analyzer`라는 도구를 도입함으로써 애플리케이션을 구성하는 각 패키지나 컴포넌트의 용량을 시각적으로 확인할 수 있습니다. 기사에서는 여러 날짜 처리 라이브러리를 비교하여 동일한 기능이라도 라이브러리에 따라 번들 크기가 크게 다른 것을 실제 예로 보여주며, 라이브러리 선정의 신중한 판단이 성능에 기여한다고 결론짓고 있습니다.

결론적으로, 이러한 방법들은 Next.js를 채택하고 있는 사이트라면 바로 도입할 수 있는 것이며, 성능 개선에 있어서는 LightHouse CI와 같은 도구로 현상을 수치화·가시화하는 것이 필수적이라고 마무리하고 있습니다.