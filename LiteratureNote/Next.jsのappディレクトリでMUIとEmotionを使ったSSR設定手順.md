# Next.jsのappディレクトリでMUIとEmotionを使ったSSR設定手順

Next.jsのApp Router環境で、MUIとEmotionライブラリを用いたサーバーサイドレンダリング（SSR）を設定する具体的な手順を解説した技術記事です。`EmotionRegistry`と`ThemeRegistry`という2つのクライアントコンポーネントを作成し、ルートの`layout.tsx`に組み込む方法が紹介されています。重要なのは、`'use client'`ディレクティブを使い、クライアントサイドでのみ動作する処理を適切に分離することです。

https://zenn.dev/aki1990/articles/624f258a65dd3c