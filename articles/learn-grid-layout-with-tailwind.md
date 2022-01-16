---
title: "Tailwind CSSで理解するGrid Layout"
emoji: "🗽"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [tailwindcss, grid, css]
published: true
---

## はじめに

CSS の Grid Layout についての解説です。
Grid Layout の使い方をサクッと知りたい人向けに書いています。

:::message
Tailwind CSS で書いているので、適宜置き換えて読んでくだだい
::::

## Grid の使い方

Tailwind CSS のクラスの紹介

### grid

親要素に指定。flex と同じような使い方。

### grid-cols-{n}

(Grid Template Columns)

n 個のカラム(横の要素数)をもつグリッドかを指定。`grid`と一緒に、親要素に指定。

### col-span-{n}

(Grid Column)

要素自体の幅。子要素に指定。

```tsx:SampleComponent.tsx
<div className="grid grid-cols-3">{/* 3カラムのグリッドであることを指定 */}
  <div className="col-span-1">1</div1>
  <div className="col-span-2">2</div1>{/* この要素で2カラム分使っている */}
  <div className="col-span-1">3</div1>{/* ここから下の段に改行される */}
  <div className="col-span-1">4</div1>
</div>
```

### gap-{n}

n スペース分の余白を要素と要素の間にもたせるか。親要素に指定。
普通に要素感の余白の大きさです。
`gap-[10px]`みたいな使い方もできるはず(JIT モードなら)

## まとめ

上記プロパティをつかえるだけでも、だいぶ便利になると思います。

↓ こちらのドキュメントもわかりやすいので、他のプロパティを知りたい方は見てみると良いと思います！

https://tailwindcss.com/docs/grid-template-columns

また、自分の時間があるときに他のプロパティの解説も追記しようと思います。

これであなたも Grid Layout マスター!! 😎
