---
title: "動的サブドメインに対応したアプリケーションを開発する"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, nextjs, vercel, subdomain]
published: false
---

## はじめに

最近、ユーザーからの入力に応じた動的サブドメインで表示する Web アプリケーションを開発しており、それによって得られた知見を記していこうと思います。

イメージ的には、Zenn では

```
zenn.dev/<ユーザー ID>
```

になるのに対し、hoge.app というドメインをとっていた場合、

```
<ユーザー ID>.hoge.app/
```

となるようなイメージです。

（以降、説明のために `hoge.app` というドメインを取得している、というていで進めます。）

このような Web アプリケーションを、`Next.js` で開発する際の参考になれば幸いです。

:::message
最近 catnose さんが https://zenn.dev/catnose99/articles/6c9851560c132e のような記事を執筆されていましたが、こちらはカスタムドメインの対応が難しいという話で、今回自分が書いている記事は動的サブドメインの対応になります。
:::

## 実際に

具体例を交えて紹介します。

まず、Next.js で基本的なルーティングをしようと思ったら

```
- pages/
  - index.tsx
  - about.tsx
  - _app.tsx
```

のようにするのに対し、今回のサブドメイン対応する場合は

```
- pages/
  - _sub // 以下サブドメインのページ 例）`mayo.hoge.app`
    - [subdomain]
      - index.tsx
  - _root // 以下サブドメインを含まないルートドメインのページ `hoge.app` || `www.hoge.app`
    - index.tsx
  _app.tsx
```

のようにします。当然このままだとサブドメイン等では動かないので、middleware を使ってパスの書き換えをしていきます。

```typescript:/middleware.ts
import type { NextMiddleware } from "next/server";
import { NextResponse } from "next/server";
import { CONFIG } from "src/constants/config";
import { PUBLIC_FILES_REGEXP } from "src/constants/regexp";

export const middleware: NextMiddleware = (req) => {
  const {
    headers,
    nextUrl: { pathname }, // pathname = pages 以下のパス名
    nextUrl,
  } = req;
  if (
    pathname.startsWith("/_next") || // exclude Next.js internals
    pathname.startsWith("/api") || //  exclude all API routes
    pathname.startsWith("/static") || // exclude static files
    PUBLIC_FILES_REGEXP.test(pathname) // exclude all files in the public folder
  )
    return NextResponse.next();

  const hostname = headers.get("host") || CONFIG.HOST;

  // サブドメインでない場合は/_root へ リライト
  if (hostname === CONFIG.HOST) {
    nextUrl.pathname = `/_root${nextUrl.pathname}`;
    return NextResponse.rewrite(nextUrl);
  }

  const subdomain = hostname.replace(`.${CONFIG.HOST.replace("www.", "")}`, "");

  // サブドメインで始まる場合は/_subへ リライト
  nextUrl.pathname = `/_sub/${subdomain}${nextUrl.pathname}`;
  return NextResponse.rewrite(nextUrl);
};

```

↑ の middleware 内の実装をみてもらえば分かる通り、

- サブドメインからのリクエストでない場合は `pages/_root/` を参照 （www 始まりの場合は例外でこっち）
- サブドメインからのリクエストの場合は `pages/_sub/[subdomain]/` を参照
- その他静的ファイル、API Routes へのリクエストはそのまま返却

というような感じになっています。

動的サブドメインに対応した Web アプリ、ときくとあまり実装のイメージがつかないと思いますが、Next.js であれば、`dynamic routing` と `middleware`を使用して簡単に実装することが可能です。

## 考慮すべきポイントなど

開発している中で、動的サブドメインに対応したアプリを開発する際に考慮する必要があるポイントをつらつら書き残していきます。

- 認証
  もし JWT などを LocalStorage に保存している or 保存しようとしている場合、ルートドメインやサブドメイン間では共有できないので、ひと工夫する必要があります。
  iframe や postMessage を使って別ドメインと通信して LocalStorage 等の値を読み書きするか、認証情報の保存先を Cookie にし、domain 属性でサブドメインでも共有できるようにする、など。自分の場合は後者で対応しました。

- 開発環境
  今回の場合は Vercel を使用していますが、別で開発環境用ドメインを購入する以外の方法がわからず、別途ドメインを購入して対応しました。アプリ内では独自のデプロイ用環境変数を使用して環境の判別をしています。

- 予約語対応等
  今回自分が開発していたアプリは、ユーザーの入力値をサブドメインにするという機能のため、www や記号をはじくなどのバリデーションが必要です。サブドメインに限らず、ユーザーからの入力値をそのまま使用したい場合には考慮する必要がありますね。

- ページ間のルーティング
  例えば `foo.hoge.app`から`bar.hoge.app`へページ内遷移をしたいとなった場合、Next.js の Link コンポーネントは // TODO: 要検証

### 参考にしたもの

https://github.com/vercel/platforms

↑ こちらの Vercel 公式のデモアプリのソースを参考にしました。Vercel の API を用いたカスタムドメインにも対応していたので、気になる方は一度見てみてください。

## まとめ

最後までご覧頂きありがとうございました。
質問や指摘、改善点など歓迎ですので、ぜひコメントしていただければと思います 🙏
