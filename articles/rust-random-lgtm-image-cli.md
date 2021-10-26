---
title: "RustでランダムなLGTM画像を取得するCLIツールを作ってみた"
emoji: "🥴"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [rust]
published: false
---

## 前提

- Rust やプログラミングの基礎知識、環境構築等

### 今回作るもの

タイトル通り、ランダムな LGTM 画像をダウンロード、及び生成する CLI ツールを作成しました。

#### 具体的な内容

- コマンドを実行すると、2 枚の LGTM 画像を生成する。
- 1 枚は LGTMOON さんからランダムに取得。
- もう一枚は PixabayAPI から画像を取得し、LGTM の文字を描画して生成。
- PixabayAPI の検索ワードは指定できるようにする。指定がなければランダム。(予定)

:::message
LGTMOON さんのサービスと、PixabayAPI(どちらも無料)を利用しています
:::
