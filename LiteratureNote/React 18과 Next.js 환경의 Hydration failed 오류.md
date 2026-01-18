# React 18과 Next.js 환경의 Hydration failed 오류

**Original URL**: https://zenn.dev/onikun/articles/28fb8de056c89a

**Summary**:
React 18과 Next.js 환경에서 발생하는 "Hydration failed" 오류에 대해 설명하는 글입니다. Hydration은 Next.js가 생성된 HTML을 최소한의 JavaScript 코드와 연결하여 페이지를 상호작용 가능하게 만드는 과정입니다. 이 오류는 서버에서 렌더링된 초기 UI가 클라이언트에서 렌더링된 것과 일치하지 않을 때 발생하며, `p` 태그 안에 `div` 태그를 사용하는 것과 같은 잘못된 HTML 구조가 주된 원인입니다. React 17에서는 이러한 잘못된 HTML이 경고로만 나타났지만, React 18부터는 "Hydration failed" 오류를 발생시킵니다. 해결책은 올바른 HTML 태그 사용을 확인하는 것이며, 문제가 지속되면 React 17로 다운그레이드하는 것을 고려할 수 있습니다.

**Date Created**: 2026-01-15