---
title: "作成,更新,削除といった処理のローディング中には、buttonタグのdisabled属性をtrueにしよう"
emoji: "😷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [html, graphql, react, nextjs]
published: true
---

## button タグの disabled 属性とは

簡単に言うと、HTML のボタンタグを無効化するかどうか、です。
disabled はには true/false のどちらかの値を付与でき、disabled 属性が true になっている間はボタンが無効化(クリック不可)されます。(デフォルトは false)

↓ 参考
https://developer.mozilla.org/ja/docs/Web/HTML/Element/button

## 今回伝えたいこと

React や Next.js など、JavaScript で作成、更新、削除といった処理のローディング中にボタンを disabled=true にしないと、ボタンの連打で予期せぬ挙動やエラーなどが起きたりする可能性があるので、注意しようという話です。

### 具体的な実装例

React(Next.js), GraphQL (Apollo Client) の場合での、ローディング中にボタンを無効化する例です。
※ react-host-toast も使用していますが、特に関係ないので無視してもらって大丈夫です。

```tsx:SampleComponent.tsx
import toast from "react-hot-toast";
import { useSampleMutation } from "src/graphql/schemas";
import { useState } from "react";

export const SampleComponent: React.VFC = () => {
  // 何らかの更新する関数 (GraphQL Code Generatorで自動生成された関数)
  const [mutation, { loading: isLoading }] = useSampleMutation();
  // inputのstate
  const [inputVal, setInputVal] = useState("");

  // inputの値が更新されるたびにstateも更新
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setInputVal(e.target.value);
  };

  // 作成用関数
  const handleSubmit = async (e: React.ChangeEvent<HTMLFormElement>) => {
    e.preventDefault();
    const toastId = toast.loading("作成中");
    try {
      const { errors } = await mutation({
        variables: { title: inputVal },
      });
      if (errors) throw errors;
      toast.success("成功", { id: toastId });
      setInputVal("");
    } catch (error) {
      toast.error("失敗", { id: toastId });
      console.error(error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={inputVal} onChange={handleChange} />
      <button
        type="submit"
        disabled={isLoading}
      >
        作成
      </button>
    </form>
  );
};
```

上記の例のようにローディング中は disabled にしないと、作成ボタンを連打した際に何個も作成出来てしまうので気をつけて下さい。
無駄にリクエスト数を増やさないなどのメリットもあると思うので、まだやっていない方は是非このようにすることをおすすめします。

:::message
REST API の場合の例がなくてすみません。必要であれば追記します。
また、もっと良い書き方があったら教えて下さい 🙏
:::

disabled の間(isLoading が true の間)はボタンの UI を変更したりしても良いと思います。

## まとめ

- 作成、更新、削除といった処理中は button タグを disabled にしよう
- ローディング中はボタンの UI を変更したり、今回の例のようにトーストを表示すると良いかも

以上簡単な内容でしたが、最後まで見て頂きありがとうございました！
誰かの役に立てば幸いです。
また、見にくいところや間違いなどがあればご指摘下さい。
