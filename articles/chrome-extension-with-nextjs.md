---
title: "Next.js, TypeScript, Tailwind CSS で快適に始める（かもしれない）Chrome拡張機能開発"
emoji: "🔌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [chrome, extension, nextjs, typescript, tailwindcss]
published: true
---

## はじめに

こんにちは。実務が始まって 3 週間ほどのまよねーづです。
みなさんは Chrome 拡張機能の開発をしたことがありますか？

自分は過去に 1 度つくったことがあるのですが、素の `HTML / CSS / JS` のみで開発したり、海外の方の `React / TypeScript` のテンプレートをクローンして、`CSS Modules` を使って開発することしかありませんでした。

せっかくだし Chrome 拡張も自分の好きな技術を使いたいなと思い、下記スタックで今回挑戦して作ってみました。

- Next.js
- TypeScript
- Tailwind CSS

今回は上記スタックで Chrome 拡張機能を作り、その副産物として**テンプレートリポジトリを作成したので、それの紹介記事**です。

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

#### Chrome 拡張開発 入門記事

- https://www.tackn.jp/post-3806.html
- https://roy29fuku.com/web/browser/google-chrome-extension-tutorial/

https://developer.chrome.com/docs/extensions/reference/
https://support.google.com/chrome/a/answer/2714278?hl=ja

:::message alert
1 つ注意点として、manifest v2 と v3 で大きく変わる箇所があるので、そこだけ注意しながら開発してください。今回は v3 で作成しています。
:::

## [テンプレートリポジトリ](https://github.com/mayone-du/chrome-extension-template-with-nextjs/)の使い方

[README](https://github.com/mayone-du/chrome-extension-template-with-nextjs/#readme) にも記載してますが、ここでも説明します。

- ビルドは、`npm run export`で `/extensions` 以下に出力されます。Chrome 側で拡張機能を読み込む際は `/extensions` を選択してください。
- `/src/scripts` 以下の 2 種類のファイルが、それぞれ Chrome 拡張用のスクリプトファイルです。(`content.js` | `background.js`)
- ChromeAPI を使用する場合は、コンポーネント化して`dynamic import` で読み込み、`ssr: false`にするようにしてください。そうしないとビルド時に`chrome`が未定義だと怒られビルドできません。
- 自分で webpack の設定を書いて Chrome 用のスクリプトをバンドルしています。ここらへんのパスやフォルダ構成等をいじる際は、`/webpack` の設定も見直してください。
- 適宜 `manifest.json` や linter, formatter の設定を変えてください。
- ページ遷移は普通に Next.js のルーティングに従います。

## 欠点

実は、このリポジトリは大きな欠点があり、**ChromeAPI を使う場合**、開発中の動作確認方法が今の所（2021/11/23 日現在）毎回ビルド(npm run export)するしかなく、効率が悪いです。タイトル詐欺ってごめんなさい。

**理由としては ChromeAPI が window オブジェクトに存在しないため**なので、ローカルの場合は ChromeAPI を使わずにうまいことやれば出来なくはないです。

:::message
ChromeAPI を使用しない場合であれば、普通に`npm run dev`で確認できます
:::

もし本格的に Chrome 拡張の開発をしたことがある人や、なにか良い方法があれば是非コメントや PR 等いただけると嬉しいです 🙏

https://github.com/mayone-du/chrome-extension-template-with-nextjs/

## 参考にさせていただいた記事

- https://qiita.com/yushimatenjin/items/8f476dd018550922f1e9
- https://zenn.dev/cti1650/articles/81016618ec5a87

## まとめ

- `/extensions`が拡張機能のディレクトリになる
- `/src/scripts`が Chrome 用のスクリプトファイルになる
- Chrome 用のスクリプトファイルと通信したい場合は、`dyamic import`を使ってコンポーネント側で `sendMessage` などする。
- 動作確認方法や、ディレクトリ構造の変更に注意。

Chrome 拡張は作れるとなにかと便利だし、できることの幅も広がって楽しいので、ぜひ今回紹介したテンプレートを使用して Chrome 拡張の開発に挑戦してもらえればと思います！

https://github.com/mayone-du/chrome-extension-template-with-nextjs

今回のテンプレート自体にもまだまだ改善の余地はあると思うので、気が向き次第改善し続けます。

最後までご覧頂きありがとうございました 🙏
コメントやご指摘等も大歓迎ですので、是非ご提案、ご意見をお待ちしております。
