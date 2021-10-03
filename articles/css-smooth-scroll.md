---
title: "CSS1行でスムーズスクロールを実装する"
emoji: "🪡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [html, css]
published: true
---

## 実装

↓ こちらのツイート通り、CSS に
https://twitter.com/mayo1201blog/status/1327475462096838662

```css:style.css
html {
  scroll-behavior: smooth;
}
```

とするだけで、ページ内リンクをクリックした際にスムーズスクロールになります ✨

## おまけ

更にスムーズスクロールについて
html タグか何かに、 CSS の scroll-padding-top というプロパティで、ヘッダーの高さ分指定してあげるとスクロール後の位置を調整できるようになります。追従するヘッダーのせいでスクロール後の位置がおかしい！って場合にはこちらを試してみて下さい 👍
