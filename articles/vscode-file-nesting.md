---
title: "フロントエンドエンジニアに勧めるvscode の file nesting"
emoji: "📂"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [vscode, frontend]
published: true
---

## File Nesting is なに

その名の通り、ファイルがネストされます。  
どういうことかというと、こんな感じです ↓

![](/images/vscode/file-tree.png)

昨今のフロントエンドでは 1 つのコンポーネントに Storybook や CSS、test などのファイルが存在する場合があるとおもいます。

自分の場合は Storybook のファイルのみでしたが、これだけでも正直、

- ツリー構造が見にくい
- ファイル名のタイポに気づきにくい

などがあり辛かったです。

そんなときに VSCODE の File Nesting という機能が出たことをきき、試したところかなり快適になったので紹介しました。

## 具体的な設定方法

1. まずエディタを開いて、 ⌘, で設定を開きます

![](/images/vscode/file-nesting-initial.png)

2. 検索窓に「file nesting e」と入力します
3. 下記画像のように、ネストする条件と、機能を有効化に設定します

![](/images/vscode/file-nest-config-edited.png)

自分は `tsx` に対しての設定のみ変えてますが、ここらへんはお好みで。自分は以下の感じで設定しました（超適当）

```text
${capture}.*tsx, ${capture}.*ts, ${capture}.*css
```

:::message
ちなみに正規表現が使えるわけではないっぽいので注意してください
:::

以上が設定方法です。好みが分かれる機能だと思いますが、見やすくなって 快適なので、 ぜひ 1 度試してみてはいかがでしょうか
