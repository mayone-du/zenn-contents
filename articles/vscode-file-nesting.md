---
title: "フロントエンドエンジニアに勧めるvscode の file nesting"
emoji: "📂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [vscode]
published: false
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
2. 検索窓に「file nesting」と入力します
3. 下記画像のように、ネストする条件と設定します

```text
${capture}.*tsx, ${capture}.*ts, ${capture}.*css
```

![](/images/vscode/file-nest-config-edited.png)
storybook, test, css...
