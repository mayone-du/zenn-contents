---
title: "Tailwind CSSで作るスケルトンローディングUI"
emoji: "💀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [tailwindcss]
published: false
---

## はじめに

メチャクチャ簡単なものですが、Tailwind CSS でスケルトンローディングの UI を作成します。

## 前提

- CSS の基礎知識
- Tailwind CSS のセットアップ

### 最終的なソースコード

```tsx:SampleComponent.tsx
<ul className="w-full border-b">
  <li className="flex items-center w-full border-t">
    {/* 画像 */}
    <div className="w-20 h-20 bg-gray-100 animate-pulse"></div>
    {/*  */}
    <div className="mx-auto w-5/6">
      {/* タイトル */}
      <div className="w-full h-4 bg-gray-100 animate-pulse"></div>
      {/* 説明文 */}
      <div className="my-1 w-full h-2 bg-gray-100 animate-pulse"></div>
      <div className="my-1 w-full h-2 bg-gray-100 animate-pulse"></div>
      {/* 時刻 */}
      <div className="flex justify-between items-center">
        <div className="my-1 w-24 h-2 bg-gray-100 animate-pulse"></div>
        <div className="my-1 w-10 h-2 bg-gray-100 animate-pulse"></div>
      </div>
    </div>
  </li>
</ul>
```

https://tailwindcss.com/docs/animation

## おまけ

ついでに他のアニメーションも実装

#### スピナー
