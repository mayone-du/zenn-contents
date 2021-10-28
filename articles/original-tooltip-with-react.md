---
title: "超シンプルなツールチップのコンポーネントを自作した"
emoji: "💡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, nextjs, tailwindcss]
published: false
---

## はじめに

Next.js でシンプルないい感じのツールチップコンポーネントを自作したので、作成する方法のメモになります。

### 前提

- React / Next.js / TypeScript の基礎理解、環境構築済み

### 今回作るもの

gif を掲載

## 本文

ソースコードの表示後にツールチップの説明を記載します。

### 最終的なソースコード

```tsx:Tooltip.tsx
import { memo, useRef } from "react";

// ツールチップ内に表示するためのprops
type Props = {
  tooltipText: string;
};

// ツールチップ
export const Tooltip: React.FC<Props> = memo((props) => {
  const ref = useRef<HTMLDivElement>(null);

  // マウスが乗ったらツールチップを表示
  const handleMouseEnter = () => {
    if (!ref.current) return;
    ref.current.style.opacity = "1";
    ref.current.style.visibility = "visible";
  };
  // マウスが離れたらツールチップを非表示
  const handleMouseLeave = () => {
    if (!ref.current) return;
    ref.current.style.opacity = "0";
    ref.current.style.visibility = "hidden";
  };

  return (
    <div className="flex relative items-center">
      <div
        className="flex before:block absolute before:absolute top-full before:-top-1 left-1/2 before:left-1/2 invisible z-10 before:z-0 items-center py-[2px] px-2 mx-auto mt-2 before:w-2 before:h-2 text-xs text-white whitespace-nowrap before:bg-black bg-black rounded transition-all duration-150 transform before:transform before:rotate-45 -translate-x-1/2 before:-translate-x-1/2"
        ref={ref}
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
      >
        {props.tooltipText}
      </div>
      <div onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave}>
        {props.children}
      </div>
    </div>
  );
});

Tooltip.displayName = "Tooltip";

```

最初に useRef でツールチップテキストの div タグへの ref を取得します。
その後 handleMouseEnter, handleMouseLeave でツールチップの表示・非表示を制御します。
ref が存在すれば、ツールチップのテキストを style で表示・非表示にするだけです。

#### 使い方

使う側で hover したときにツールチップを表示したい要素を<Tooltip>children</Tooltip>で囲みます。

## まとめ

最後までご覧頂きありがとうございました。
