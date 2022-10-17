---
title: "フロントエンド開発に便利なChrome拡張 LocatorJSの紹介"
emoji: "🏎"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [react, frontend, vscode, vue]
published: true
---

## LocatorJS is 何

**option + 画面クリックでコードジャンプできる Chrome 拡張** です

https://www.locatorjs.com/

ストア ↓

https://chrome.google.com/webstore/detail/locatorjs/npbfdllefekhdplbkdigpncggmojpefi

![](/images/locatorjs-introduction.gif)
↑ このような感じで option キーを押しながらクリックすると対象のコンポーネントへコードジャンプできます。（サンプルは LocatorJS のためジャンプ先が GitHub になっている）

似たようなものに

https://github.com/ericclemmons/click-to-component

などありますが、明確な違いとしてパッケージをインストールせず、Chrome 拡張を入れるだけで使える、というのがあります。

また、実務で一度 ↑ をためしたんですが、いくつか不具合を踏んだり一部のコンポーネントの挙動が不安定になる、別に求めていないメンバーもいる、などのつらさがありました。

LocatorJS であればそれらも解決できるのが魅力です。

## 注意点

あまり多くはないとは思いますが、Dev Container 等でフロントエンド開発をしているとおそらくうまく動作しません。他のツールも同様だと思います。

## まとめ

個人的にかなり重宝している拡張機能なので、ぜひ色んな人に一度使ってみてほしいなと思い、紹介しました。
普段の使用はもちろんですし、新しくチームメンバーが増えるタイミングなどもかなり刺さるのではないかなと思っています。

最後までご覧頂きありがとうございました 👋
