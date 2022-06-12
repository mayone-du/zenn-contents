---
title: "今話題のUIライブラリ「Mantine」"
emoji: "🐟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [mantine, react, nextjs, ui, library]
published: false
---

## はじめに

OSS の React 専用 UI ライブラリ 「Mantine」 の簡単な紹介・軽く触ってみた感想記事です。
普段から React や TypeScript を使っている方を対象にしています。

https://mantine.dev/

### この記事で書くこと

- 特徴、メリット・デメリット（というか軽く触った感想）
- Mantine のセットアップ
- form 系コンポーネント、カスタムフックの使い方

:::message
Mantine の深い知見があるわけではないので、Mantine の雰囲気を知ってもらえればと思います
:::

## Mantine の特徴

Mantine とは、すでに述べているとおり、OSS の **React 専用の UI ライブラリ**です。

### メリット

1. コンポーネントの種類が豊富

UI ライブラリなので当然といえば当然なのですが、コンポーネントの種類がとても豊富です。  
特に form 系のコンポーネントが充実していて、大体の用途で間に合うと思います。

また、`DatePicker` や `Notification`、`RichTextEditor` など、専用の別ライブラリを入れて使うことが多いようなコンポーネントも公式のプラグインとして存在しています。

2. ダークモード対応

最近の UI ライブラリであればダークモード対応していることは多いと思いますが、Mantine も例にもれずダークモードに対応しています。

`ColorSchemaProvider`を使用することで動的に変更することもできます。詳しくは下記ドキュメントを参照してください。

https://mantine.dev/theming/dark-theme/#colorschemeprovider

3. カスタマイズしやすい

コンポーネントのスタイリング用の API が公開されているのはもちろんですが、「内部」のコンポーネントのスタイルも簡単にカスタマイズできるようになっています。

たとえば、セレクトボックスで例えると基本は

4. form の実装が楽

Mantine によって`useForm`という hooks が提供されており、form 系のコンポーネントの扱いが楽です。  
form の state 管理ライブラリとして有名なものに`ReactHookForm`があると思いますが、これを使おうと思うと自分が使ってる UI ライブラリによって実装方法が変わ

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

以上が Mantine の紹介になります。
最後までご覧頂きありがとうございました。
