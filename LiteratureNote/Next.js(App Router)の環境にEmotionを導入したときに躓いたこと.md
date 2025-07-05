# Next.js(App Router)の環境にEmotionを導入したときに躓いたこと

Next.jsのApp Router環境にCSS-in-JSライブラリのEmotionを導入した際の注意点をまとめた技術記事です。App RouterのデフォルトであるReact Server Components（RSC）にEmotionがまだ完全対応していないため、Emotionを利用するコンポーネントには`'use client'`を記述する必要がある、という問題点に焦点を当てています。また、その背景にあるJSX pragmaという技術的な概念についても解説しています。

https://zenn.dev/gekitenius/articles/approuter_with_emotion_202401