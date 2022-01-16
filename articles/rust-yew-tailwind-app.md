---
title: "RustとTailwind CSSでブログつくってみた"
emoji: "🎡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["rust", "yew", "wasm"]
published: true
---

## はじめに

`Rust`の`Yew`という Web フロントエンドのフレームワークで、自分のブログのフロントを書いてみました！
スタイリングには Tailwind CSS を使用しています。

実際にできたもの ↓
https://mayone-du.github.io/yew-blog

リポジトリ ↓
https://github.com/mayone-du/yew-blog

**この記事で書くこと、書かないこと**
書くこと

- 普段 React や TypeScript で開発している自分が思った感想
- 詰まったところ

かかないこと

- Rust の基礎知識や、Yew での書き方の詳しい解説など

Rust で Web フロントを書くときの雰囲気を知ってもらえるきっかけになればと思っています。

:::message
Rust 初学者、というか、「実務経験 3 ヶ月にも満たないエンジニアが書いている記事」ということをあらかじめご承知おきください ﾍﾟｺﾘ
:::

### Rust で Web フロントを書く際の主な選択肢

- Yew
  - https://github.com/yewstack/yew
- Seed
  - https://github.com/seed-rs/seed
- Dioxus
  - https://github.com/DioxusLabs/dioxus

自分が聞いたことあるのだとこのくらいです。`Yew`が`18k`と一番スター数が多かったので、`Yew`を採用しました。
`html!`マクロで`JSX`のような記法ができるのは便利でした。
それ以外の基本的な構文などは、`iced`という GUI アプリのフレームワークに似ていました。

https://github.com/iced-rs/iced

どっちも`Elm`アーキテクチャを参考にしてるとかそんな感じでした。
そこら辺はあまり詳しくないので、間違ってたらごめんなさい 🙏

https://guide.elm-lang.jp/architecture/

## 今回作ってみた感想など

- WASM を生成しているということを忘れちゃいけない
  - `std::fs::read_dir`を使おうとしてコンパイルは通るけど、ランタイムで動かなくて詰まった。結局は`wasm`が生成されてブラウザ上で実行されているので、`wasm`を生成していることを忘れないようにしないと詰まることがあるなと思った。(結局日本の Rust コミュニティで解決していただいた。詳しくは自分のスクラップへ。)
- Rust の Web フロントのエコシステムはまだまだ貧弱、というか少ない
  - 個人的には、本気で Web アプリ作るとしたら Rust でやるメリットはそこまでないように感じた。主にエコシステムや、慣れの面で。
  - 普段低レイヤな部分を自分がさわることがなく、自分はライブラリがないと何もできないマンなので、つよつよな人なら全然いいのかも？
- React のように`Functional Component`で`hooks`をからめた書き方もあるっぽい。さっき紹介した`Dioxus`でも React ライクにかけるらしいとの噂を聞いたので、普段 React をさわってる人はそちらもありかも？
- 複雑なのはしんどそうだけど、簡単なものなら普通に`Rust`で作れる
- VSCODE の Tailwind のプラグインの補完とか、`html!`マクロ内のブロックにフォーマットが効かないの少ししんどい

### 実際のコードを一部紹介

↓ 記事一覧用の JSON を返す URL へリクエストを投げ、それぞれの要素を一覧表示するコンポーネント

```rust:article_list.rs
use crate::client::{
  fetch::fetch_row_text,
  state::{FetchMessage, FetchState},
};
use crate::constants::vars::ARTICLE_LIST_META_URL;
use crate::meta::data_list::ArticleMetaDataList;
use crate::routes::app_routes::AppRoutes;
use yew::{html, Component, Context, Html, Properties};
use yew_router::components::Link;

#[derive(PartialEq, Properties)]
pub struct Props;

pub struct ArticleList {
  response: FetchState<String>,
}

impl Component for ArticleList {
  type Message = FetchMessage;
  type Properties = Props;

  fn create(ctx: &Context<Self>) -> Self {
    // コンポーネントの初期化時にFetchを開始するmessageを送信
    ctx.link().send_message(FetchMessage::FetchStart);
    Self {
      response: FetchState::NotFetching,
    }
  }

  fn update(&mut self, ctx: &Context<Self>, msg: Self::Message) -> bool {
    match msg {
      FetchMessage::SetFetchState(fetch_state) => {
        self.response = fetch_state;
        true
      }
      FetchMessage::FetchStart => { // FetchMessage::FetchStartが送られると非同期でHTTPリクエストが送られる
        ctx.link().send_future(async {
          match fetch_row_text(ARTICLE_LIST_META_URL).await {
            Ok(md) => FetchMessage::SetFetchState(FetchState::Success(md)),
            Err(err) => FetchMessage::SetFetchState(FetchState::Failed(err)),
          }
        });
        ctx
          .link()
          .send_message(FetchMessage::SetFetchState(FetchState::Fetching));
        false
      }
    }
  }

  fn view(&self, _ctx: &Context<Self>) -> Html {
    let loading = html! {
      <div class="animate-pulse bg-gray-300 w-full h-8"></div>
    };
    // stateに応じて出し分け
    match &self.response {
      FetchState::NotFetching => loading,
      FetchState::Fetching => loading,
      FetchState::Success(data) => {
        let json_data: ArticleMetaDataList = serde_json::from_str(&data).unwrap(); // 文字列を独自定義したstructのvec型に格納し、mapで展開
        html! {
          <ul class="grid lg:gap-6 gap-4 lg:grid-cols-3 md:grid-cols-2 grid-cols-1">
            {
              json_data
              .map(|meta| {
                if meta.is_published {
                  html! {
                    <li class="col-span-1 border border-gray-200 rounded-lg shadow-sm overflow-hidden bg-white transition-all hover:bg-gray-50 hover:-translate-y-1" title={meta.title.clone()}>
                      <Link<AppRoutes> classes="block" to={AppRoutes::Article { id: meta.created_at.clone() }}>
                        <div class="lg:text-7xl text-5xl py-6 text-center bg-blue-100 border-b border-gray-100">{meta.emoji}</div>
                        <div class="p-4">
                          <h5 class="text-lg font-bold pb-3">{meta.title}</h5>
                          <p class="text-sm text-gray-500 pb-3">{meta.description}</p>
                          <small class="block text-sm text-right">{&meta.created_at}</small>
                        </div>
                      </Link<AppRoutes>>
                    </li>
                  }
                } else {
                  html! {}
                }
              })
              .collect::<Html>()
            }
          </ul>
        }
      }
      FetchState::Failed(err) => html! { err },
    }
  }
}

```

↓Props で文字列を受け取り、HTML にパースして表示するコンポーネント

```rust:markdown.rs
use crate::components::row_html::RawHTML;
use pulldown_cmark::{html::push_html, Parser};
use yew::{html, Component, Context, Html, Properties};

// PropsでString型のmarkdownデータを受け取る
#[derive(PartialEq, Properties)]
pub struct Props {
  pub markdwon_data: String,
}

pub struct Markdown;

impl Component for Markdown {
  type Message = (); // 更新系の処理はないので空
  type Properties = Props;

  fn create(_ctx: &Context<Self>) -> Self {
    Self
  }

  fn view(&self, ctx: &Context<Self>) -> Html {
    // テキストデータをHTMLにパース
    let parser = Parser::new(&ctx.props().markdwon_data);
    let mut html_buf = String::new();
    push_html(&mut html_buf, parser);
    html! {
      <article class="prose prose-slate mx-auto">
        <RawHTML inner_html={html_buf} /> // HTMLを埋め込み
      </article>
    }
  }
}
```

#### できたこと

- ルーティングや HTTP リクエスト、state 管理などの基本的なこと
  - ルーティングには`yew-router`
  - HTTP リクエストには`web_sys`をつかっている
  - 非同期処理は、`Future`を`wasm_bindgen::JsFuture`に変換しながらやると良さそう？このあたりもまだなんとなくでやっていて理解が浅いです、すみません。。
- Markdown テキストの HTML への変換、メタ部分の抽出
- Tailwind CSS でのスタイリング
- GitHub Pages / GitHub Actions での自動デプロイ

ちなみに Tailwind CSS のセットアップは以下の記事を参考に進めました。

https://dev.to/arctic_hen7/how-to-set-up-tailwind-css-with-yew-and-trunk-il9

ルーティングやデータの取得は公式などのサンプルコードをもとに、自分なりにいじって書いています。
レイアウトの共通化とかも普通にできて、ディレクトリ構成は`Next.js`で開発するときのようなイメージです。
本当に簡単なことしかしていませんが、自分にはちょうど良い難易度でした。

https://github.com/mayone-du/yew-blog

#### できていないこと、やっていないこと　

- トップページ以外へ直接アクセスした際のルーティング(どちらかというと GitHub Pages の問題かも)
- API リクエスト時のサーバーデータのキャッシュ管理
- SSG など
- グローバルステートの管理(今回は必要なかったから、そもそも調べてない)
- GraphQL でのリクエスト(試したけど挫折しました w)

その他複雑なことはやっていません。

#### 疑問に思ったこと

開発してて思ったことです。知ってる方いたら教えていただけると嬉しいです！！

- Rust でモジュールに分割するときは、各ディレクトリで`mod.rs`で`pub mod`キーワードでモジュールを定義し、`main.rs`で読み込んで使えるようにするのが主流？

## まとめ

- 1 から Rust でやるのはまだまだしんどい。自分の習熟度や、エコシステムなど。
- ただ、個人的にはこれからも追い続けたい。Rust 書いてる俺、かっけー感あって良い(←)
  - 真面目な話、これから流行るようであればなるべく早く触れていつでも使えるようにしときたい。
- Web における Rust のユースケースは、よく言われているようにおもい処理とか、一部を Rust で書くみたいな感じになるのかなと思った。すぐにすべて Rust に置き換わるとかはまぁ無いだろうなって思った。
- Rust の`match`式便利
- コミュニティの方優しいしわかりやすく説明してくれてありがたかった

大した記事ではないですが、最後までご覧頂きありがとうございました。
間違いや改善点等あれば、ご指摘いただけると幸いです！！
