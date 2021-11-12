---
title: "JavaScriptでオブジェクトのkeyとvalueを同時に回す方法"
emoji: "😵‍💫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [javascript, typescript]
published: true
---

## はじめに

JavaScript で、オブジェクトの key と value を同時に回す方法を紹介します。

:::message
ts と書いてますが、TS 要素ないので普通に JS しか知らない方でも使えます 👍
:::

※おまけで React / Next.js でも使えるサンプルコードを追記しました。

## 本文

↓ サンプル

```ts:sample.ts
const sampleObj = {
  a: "hoge",
  b: 2,
  c: null,
  d: ['foo', 1, undefined]
};
for (const [key, value] of Object.entries(sampleObj)) {
  console.log(key, value);
  // a hoge
  // b 2
  // c null
  // d ['foo', 1, undefined]
}
```

ここでやってることを紐解きます。

1. Object.entries(obj)で、[key, value] という配列を返す

```ts:destructuring.ts
const sampleObj = {
  a: "hoge",
  b: 2,
  c: null,
  d: ['foo', 1, undefined]
};
const [key, value] = Object.entries(obj)[0];
console.lgo(key, value);
// a hoge
```

#### 参考

- https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
- https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/entries

2. for of で、1 で返る値を 1 ループごとに key, value へ代入

```ts:forOf.ts
for (const val of ["hoge", 3]) {
  console.log(val);
}
// hoge
// 3
```

#### 参考

- https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/for...of

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/for...of

この 1, 2 を組み合わせることで、最初のサンプルコードのような結果になります。
どのプロパティの値がどうなっているかをチェックしたいときとかに便利です。

## おまけ

_React / Next.js で使う方法_

```ts:sample.tsx
const sampleObj = {
  key1: "value1",
  key2: "value2",
  key3: "value3",
};
const SampleComponent: React.VFC = () => {
  return (
    Object.entries(sampleObj).map(([key, value]) => {
      return (
        <div key={key}>
          {key} : {value}
        </div>
      );
    })
  )
}
```

## まとめ

for of と分割代入を組み合わせてるだけで、この 2 つがわかってれば難しくないと思うので、難しいと感じた方はまずは for of と分割代入を理解してみると思います。
メモみたいな感じでサクッと書いてしまったので、おかしいところやわかりにくいところがあればご指摘下さい 🙏
最後までご覧いただきありがとうございました！
