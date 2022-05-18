---
title: "TypeScriptのUtility Typesの実用例"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [typescript]
published: false
---

## Record

string enum をもとに辞書を作成する

```ts
enum Color {
  Red,
  Green,
  Blue,
}

const COLOR_MAP: Record<Color, string> = {
  [Color.Red]: "red",
  [Color.Green]: "green",
  [Color.Blue]: "blue",
};
```

## Omit

特定のプロパティを除外する

```ts
Omit<Obj, "__typename">

```
