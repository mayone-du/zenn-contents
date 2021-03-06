---
title: "今話題のUIライブラリ「Mantine」"
emoji: "🐟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [mantine, react, nextjs, ui, library]
published: false
---

## はじめに

React 専用の UI ライブラリ 「Mantine」 の簡単な紹介・軽く触ってみた感想記事です。
普段から React や TypeScript を使っている方を対象にしています。

https://mantine.dev/

### この記事で書くこと、書かないこと

### 書くこと

- 特徴、メリット・デメリット（というか軽く触った感想）
- Mantine のセットアップ
- form 系コンポーネントの使い方

### 書かないこと

- 他の UI ライブラリとの比較
- 詳しいコンポーネントや hook の解説
- Mantine の深い知見

<!-- 筆者の経験としては、ガッツリ触ったことのある UI ライブラリなどは Headless UI や MUI くらいなので、正直他の UI ライブラリや -->

:::message alert

:::

## Mantine とは、Mantine の特徴

少し前置きが長くなりましたが、ここから Mantine の特徴や使い方を紹介します。  
Mantine とは、すでに述べているとおり、OSS の **React 専用の UI ライブラリ**です。

### メリット

1. コンポーネントがとても豊富

UI ライブラリなので当然といえば当然なのですが、圧倒的にコンポーネントの種類が豊富です。  
冒頭で他のライブラリとの比較は書かないとか言っときながらあれなんですが、他のライブラリと比べても種類が多いです。

DatePicker や Notification、RichTextEditor など、専用の別ライブラリを入れて使うみたいなことが多いようなコンポーネントも公式のプラグインとして提供されています。

その他にも、[PasswordInput](https://mantine.dev/core/password-input/)など、既存のコンポーネントを自分で拡張しないとないようなものまであるのも、とても良いと思いました。

2. ダークモード対応

デフォルトで

3. カスタマイズしやすい

コンポーネントのスタイリング用の API が公開されているのはもちろんですが、「内部」のコンポーネントのスタイルも簡単にカスタマイズできるようになっています。  
たとえば、セレクトボックスで例えると基本は

<!-- 内部のコンポーネントもスタイリング用の API が丁寧に公開されており、「ここ変えたいな」と思ったときにドキュメント見れば簡単にスタイルを上書きできるのがとても良いです。 -->

4. form 系コンポーネント の扱いが楽

Mantine によって`useForm`という hooks が提供されており、form 系のコンポーネントの扱いが非常に楽です。  
自分の辛かった体験談をあげると、`MUI`と`ReactHookForm`を一緒に使おうとした際に
また、form 系のコンポーネントも豊富。

5. 便利なカスタムフックが複数存在する

こちらも公式のプラグインとして提供されています。

- use-debounce
- use-local-storage

- Tailwind CSS との併用も可能
- デフォルトの UI に強くロックインされすぎず、カスタマイズが可能

## デメリット

1. デフォルトがアプリケーションよりの UI なので、HP や LP のようなものには向かない（当然といえば当然）

また、凝ったデザインを反映していくとなるとまたちょっと工夫が必要なのかなと思いました。

2. ボタンコンポーネントが、押下時に凹むのを制御する API がない

めっちゃ細かいですが、`Mantine`の`Button`コンポーネントは、クリックするとぴょこっと動くのですが、これを制御するための API が公開されていません。  
なので、動かないようにしたい場合は自分で CSS を上書きするしかないです。

そもそも`Button`コンポーネント自体が微妙かもという説もあり、ここらへんは自前で独自のボタンコンポーネントを用意したほうが良いかもしれません。  
（`UnstyledButton`というコンポーネントもあるので、これを拡張するのがいいのかもしれないです。）

正直、デメリットらしいデメリットはこのくらいしか浮かびませんでした。  
**個人で使ったと行っても大規模なアプリケーションで試したわけではないので、もっと習熟すると見えてくるところがあると思いますが、ご了承ください。**

## Mantine のセットアップ(with Next.js / TypeScript)

## useForm フック、TextInput コンポーネントの簡単な使い方紹介

基本的な使い方は、`ReactHookForm` と API が非常に似ている（というか Mantine が ReactHookForm をラップしているぽい）ので、`ReactHookForm`を触ったことがある人なら簡単にわかると思います。

```tsx
import { TextInput, PasswordInput } from "@mantine/core";
import { useForm } from "@mantine/form";

// フォームで扱うキーと値の型を指定
type FieldValues = {
  name: string;
  password: string;
};

const Sample: VFC = () => {
  // genericsで型を渡すことで、initialValuesやvalidate、getInputPropsで型が補完される
  const { onSubmit, reset, getInputProps } = useForm<FieldValues>({
    initialValues: { name: "" },
    validate: {
      name: (value) => {
        if (!value) return "名前を入力してください";
      },
      password: (value) => {
        if (!value) return "パスワードを入力してください";
        if (value.length < 8) return "8文字以上で入力してください";
      },
    },
  });
  const handleSubmit = onSubmit(async (values) => {
    console.log(values); // { name: string, password: string }
  });
  return (
    <form onSubmit={handleSubmit}>
      <TextInput label="名前" {...getInputProps("name")} />
      <PasswordInput label="名前" {...getInputProps("name")} />
    </form>
  );
};
```

### 番外編

自分はもともと`Tailwind CSS`が好きで`Mantine`と併用しているのですが、併用する際の Tips をまとめた記事を書きました。
もし自分みたいに併用したい方がいたらきっと役に立つと思いますので、ぜひご覧いただければと思います

## 最後に

最後までご覧頂きありがとうございました。

#####

執筆中メモ

かんたんなコンポーネントの使い方とかはやりたい

#####
