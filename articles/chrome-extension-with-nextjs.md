---
title: "Next.js, TypeScript, Tailwind CSS で快適に始めるChrome拡張機能開発"
emoji: "🔌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに

- Next.js
- TypeScript
- Tailwind CSS

上記スタックで Chrome 拡張機能を作ったので、それについての解説記事です。
テンプレートも作成したので、手軽に Chrome 拡張を作りたい人は是非ご活用ください。

https://github.com/mayone-du/chrome-extension-template-with-nextjs

:::message
改善案などあれば、PR,コメント等 お待ちしてます！
:::

### 前提

- 今回使う技術の基礎知識
- Chrome 拡張機能についての基礎知識（manifest や ChromeAPI など）

Chrome 拡張についての基礎知識は、既にわかりやすい記事があるのでそちらを参考にしてください。

### 今回作るもの

## 本文

## テンプレートリポジトリの使い方

README にも記載してますが、ここでも説明します。

- ビルドは、`npm run export`で/extension 以下に出力されます。
- /src/scripts いかの 2 種類のファイルが、それぞれ Chrome 拡張用のスクリプトファイルです。
- 自分で webpack の設定を書いて Chrome 用のスクリプトをバンドルしてるので、いじる際は webpack の設定も見直してください。
- 適宜 manifest.json や linter, formatter の設定を変えてください。
- 確認方法が今の所（2021/11/23 日現在）毎回ビルドするしかなく効率が悪いです。誰か助けてください。(おい)

## まとめ

最後までご覧頂きありがとうございました。
