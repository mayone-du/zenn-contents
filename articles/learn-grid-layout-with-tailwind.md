---
title: "Tailwind CSSで理解するGrid Layout"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [tailwindcss, grid]
published: false
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

親要素に指定。flex と同じような使い方

### grid-cols-{n}

n 個のカラム(横の要素数)をもつグリッドかを指定。`grid`と一緒に、親要素に指定。

```tsx:SampleComponent.tsx
<div className="grid grid-cols-3">
  <div>1</div1>
  <div>2</div1>
  <div>3</div1>
  <div>4</div1>{/* ←ここから下の段に改行される */}
</div>
```

### gap-{n}

n スペース分の余白を要素と要素の間にもたせるか。親要素に指定。
普通に要素感の余白の大きさです。
`gap-[10px]`みたいな使い方もできるはず(JIT モードなら)

### col-span-{n}

要素自体の幅。子要素に指定。

## まとめ

上記プロパティの使い方さえわかれば、困ることはあまりないと思います。
これであなたも CSS マスターですね 😎
