---
title: "Rustで土日祝を色付きで出力するCLIツールを作ってみた"
emoji: "👋"
type: "tech"
topics: [rust]
published: true
---

## はじめに

Rust の勉強としてタイトル通りの CLI ツールをつくったので、それを自分なりに解説した記事になります。

実務未経験者が趣味で Rust を勉強した際の最初のアウトプットなので、優しい目で見ていただけると助かります 🙏

## 前提

- Rust やプログラミングの基礎知識
- Rust の環境構築済み

### 今回作るもの

CLI から実行したら、下記画像の様に土日祝を色付きで出力するツール

![](/images/rust/get-holiday-result.jpg)

:::message alert
使用しているクレートのメンテの関係で、最新の祝日には対応出来ていません 🙏
:::

### 今回作成した GitHub リポジトリ

https://github.com/mayone-du/rust-get-holiday

## プロジェクト作成

下記コマンドで get-holiday という名前の Rust のディレクトリを作成します。

```bash:Terminal
cargo new get-holiday
```

Cargo.toml に、今回使用するクレートを追記します。

```toml:Cargo.toml
[package]
name = "get-holiday"
version = "0.1.0"
edition = "2018"

[dependencies]
chrono = "0.4.19"
colored = "2.0.0"
koyomi = "0.4.0"
```

Cargo edit をインストールしている人は、

```bash:Terminal
cargo add chrono colored koyomi
```

でもクレートの追加ができます。Cargo edit は便利なので、入れてない方はぜひ入れてみて下さい。

https://github.com/killercup/cargo-edit

#### 各クレートの簡単な説明

- chrono
  日時を扱うクレート。Rust で日時を扱う場合は、ほぼこれ。
- colored
  標準出力時にテキストを色付きにしてくれる。
- koyomi
  日本人の方が作成した、和暦を扱うクレート。メンテはされていないので、新しい祝日には対応してませんが、悪しからず。今回の例の 2021 年 10 月 11 日の体育の日はオリンピックでなくなりましたが、反映されていません。

### コーディング

先にこの後使うモジュールを use 文でスコープに追加します。

```rust:main.rs
use chrono::{Datelike, Local};
use colored::Colorize;
use koyomi::{num_days, Calendar, Date};
```

#### 独自構造体の定義

カレンダー作成のための、期間指定用の Period という構造体と new 関連関数を定義します。

new 関数では、実行した日時から年月を取得し、2021-10-01 と 2021-10-31 のような文字列を作成後、Date 型に変換しています。

```rust:main.rs
// ...use文など

// カレンダー作成時に指定する、開始日と終了日をもつ期間という意味の構造体を定義
struct Period {
    start_date: Date,
    end_date: Date,
}

// Period構造体に自身を作成するnewという関連関数を定義
impl Period {
    fn new() -> Period {
        // 現在の日時を取得
        let now = Local::now();
        let (year, month) = (now.year(), now.month());

        // 実行した月を対象とする 2021-10-01 と 2021-10-31 のような形式になる
        let start_date: String = format!("{}-{}-01", year.to_string(), month.to_string(),);
        let end_date: String = format!(
            "{}-{}-{}",
            year.to_string(),
            month.to_string(),
            // 指定した年月の最終日を返す
            num_days(year, month)
        );

        // 日付の文字列をDate型に変換
        let start_date = Date::parse(&start_date).unwrap();
        let end_date = Date::parse(&end_date).unwrap();

        // 自身を返却
        Period {
            start_date,
            end_date,
        }
    }
}
```

#### main 関数

main 関数の中身を定義していきます。

先程定義した Period を初期化します。
その後 koyomi クレートのカレンダーを、Period の期間で初期化して、Iterable なカレンダー構造体を作成します。
作成したカレンダーを for 文でまわし、日にちごとに土日祝であるかの判定をして、出力します。

```rust:main.rs
// ...use文、構造体の定義など

fn main() {
    // 実行した日時、時刻を取得
    let period = Period::new();

    // カレンダーを実行した月の期間で作成
    let calendar = match Calendar::new(period.start_date, period.end_date) {
        Ok(calendar) => calendar.make(),
        Err(err) => panic!("カレンダー初期化エラー {:?}", err),
    };

    // calendarはIterableなのでforで回し、日付を受け取るj
    for day in &calendar {
        // 曜日を判定し、土日の場合は赤・青で出力し、それ以外はスルー
        // day.weekday().japanese()はchar型を返すため、''で判定
        match day.weekday().japanese() {
            '土' => println!("{} 土", day.to_string().red()),
            '日' => println!("{} 日", day.to_string().blue()),
            // _は、上記以外の全てという意味のワイルドカード
            _ => {}
        }
        // 祝日の場合は、祝日の内容を緑で出力
        if day.holiday() != None {
            println!("{} {}", day.to_string().green(), day.holiday().unwrap(),);
        }
        // TODO: 土日と祝日がかぶっている場合の対処
    }
}
```

### 最終的なソースコード

最終的なソースコードは ↓ になります。

```rust:main.rs
use chrono::{Datelike, Local};
use colored::Colorize;
use koyomi::{num_days, Calendar, Date};

// カレンダー作成時に指定する、開始日と終了日をもつ期間という意味の構造体を定義
struct Period {
    start_date: Date,
    end_date: Date,
}

// Period構造体に自身を作成するnewという関連関数を定義
impl Period {
    fn new() -> Period {
        // 現在の日時を取得
        let now = Local::now();
        let (year, month) = (now.year(), now.month());

        // 実行した月を対象とする 2021-10-01 と 2021-10-31 のような形式になる
        let start_date: String = format!("{}-{}-01", year.to_string(), month.to_string(),);
        let end_date: String = format!(
            "{}-{}-{}",
            year.to_string(),
            month.to_string(),
            // 指定した年月の最終日を返す
            num_days(year, month)
        );

        // 日付の文字列をDate型に変換
        let start_date = Date::parse(&start_date).unwrap();
        let end_date = Date::parse(&end_date).unwrap();

        // 自身を返却
        Period {
            start_date,
            end_date,
        }
    }
}

fn main() {
    // 実行した日時、時刻を取得
    let period = Period::new();

    // カレンダーを実行した月の期間で作成
    let calendar = match Calendar::new(period.start_date, period.end_date) {
        Ok(calendar) => calendar.make(),
        Err(err) => panic!("カレンダー初期化エラー {:?}", err),
    };

    // calendarはIterableなのでforで回し、日付を受け取るj
    for day in &calendar {
        // 曜日を判定し、土日の場合は赤・青で出力し、それ以外はスルー
        // day.weekday().japanese()はchar型を返すため、''で判定
        match day.weekday().japanese() {
            '土' => println!("{} 土", day.to_string().red()),
            '日' => println!("{} 日", day.to_string().blue()),
            // _は、上記以外の全てという意味のワイルドカード
            _ => {}
        }
        // 祝日の場合は、祝日の内容を緑で出力
        if day.holiday() != None {
            println!("{} {}", day.to_string().green(), day.holiday().unwrap(),);
        }
        // TODO: 土日と祝日がかぶっている場合の対処
    }
}
```

これを

```bash:Terminal
cargo run
```

で実行すると、その月の土日祝が色付きで出力されるはずです 🥳
土日と祝日がかぶっている場合は、おそらく 2 回出力されてしまうのでそこは今度気が向いたらどうにかします（）

## 最後に

Rust で土日祝を色付きで出力する CLI ツールを作成しました。
Zenn も Rust のアウトプットも初めてなので、見にくいところやわかりにくい部分があるかもしれませんが、ご了承下さい 🤧
また、もし間違っている部分やこうしたほうがいいよなどあれば、ぜひコメントをいただけると嬉しいです！
最後までご覧いただきありがとうございました 👋
