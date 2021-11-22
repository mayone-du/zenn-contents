---
title: "Next.js, TypeScript, Tailwind CSS で快適に開発する（かもしれない）Chrome拡張機能開発"
emoji: "🔌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [chrome, extension, nextjs, react, typescript, tailwindcss]
published: false
---

## はじめに

こんにちは。実務が始まって 3 週間ほどのまよねーづです。
みなさんは Chrome 拡張機能の開発をしたことがありますか？
自分は過去に 1 度つくったことがあるのですが、素の HTML / CSS / JS のみで開発したり、海外の方の React / TypeScript のテンプレートをクローンして、CSS Modules を使って開発することしかありませんでした。
せっかくだし Chrome 拡張も下記スタックで作れたら良いよねってことで、今回挑戦して作ってみました。

- Next.js
- TypeScript
- Tailwind CSS

今回は上記スタックで Chrome 拡張機能を作り、その副産物としてテンプレートリポジトリを作成したので、それの紹介記事です。
手軽に Chrome 拡張を作りたい人は、是非今回紹介する ↓ のテンプレートリポジトリをご活用ください。

https://github.com/mayone-du/chrome-extension-template-with-nextjs

**初期の動作**
![](/images/react/chrome-extension-template-demo.gif)

:::message
最低限の使い方は README にも書いてあるので、こちらの記事は読み飛ばしてもらっても使用できると思います。
:::

### 前提

- 今回使う技術の基礎知識
- Chrome 拡張機能についての基礎知識（manifest や ChromeAPI など）

Chrome 拡張についての基礎知識は、既にわかりやすい記事があるのでそちらを参考にしてください。

## テンプレートリポジトリの使い方

README にも記載してますが、ここでも説明します。

- ビルドは、`npm run export`で /extension 以下に出力されます。Chrome 側で拡張機能を読み込む際は /extensions を選択してください。
- /src/scripts 以下の 2 種類のファイルが、それぞれ Chrome 拡張用のスクリプトファイルです。(content.js | background.js)
- 自分で webpack の設定を書いて Chrome 用のスクリプトをバンドルしています。ここらへんのパスやフォルダ構成等をいじる際は、webpack の設定も見直してください。
- 適宜 manifest.json や linter, formatter の設定を変えてください。

## 欠点

実は、このリポジトリは大きな欠点があり、開発中の動作確認方法が今の所（2021/11/23 日現在）毎回ビルド(npm run export)するしかなく、効率が悪いです。タイトル詐欺ってごめんなさい。
ですが自分も含めそのうち誰かしらいい感じにしてくれると思うので、修正でき次第記事も直します。
UI だけであれば普通に`npm run dev`すればいいのですが、ChromeAPI を使うとなるとエラーでそれどころではないので、、、
もし本格的に Chrome 拡張の開発をしたことがある人や、なにか良い方法があれば是非コメント等よろしくおねがいします。

## まとめ

最後までご覧頂きありがとうございました。
