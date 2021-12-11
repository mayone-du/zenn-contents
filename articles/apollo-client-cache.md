---
title: "Apollo Clientのキャッシュについて"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに

業務で Apollo Client を使用しており、キャッシュについて少し調べたのでメモがてら学んだことを記していきます。

### やること

- Apollo Client のデータフェッチの方法
- Apollo Client でキャッシュがどのように保存されるのかを知る
- Apollo Client を用いたキャッシュの更新

## 本文

### Apollo Client のデータフェッチの方法

### Apollo Client でキャッシュがどのように保存されるのかを知る

### Apollo Client を用いたキャッシュの更新

```tsx:sample.tsx
// data: {
//   posts: [
//     {
//       id: 1,
//       title: "タイトル1",
//       body: "本文1",
//     },
//     {
//       id: 2,
//       title: "タイトル2",
//       body: "本文2",
//     },
//   ],
// };
// クエリの結果はこのようなものだと思ってください。
const SampleComponent: React.VFC = () => {
  const { data } = usePostsQuery(); // generateされたhook
  const [addPost, { loading }] = useAddPostMutation(); // generateされたhook
  return (
    <div>
      {data.posts.map((post) => (
        <div key={post.id}>
          <h1>{post.title}</h1>
          <p>{post.body}</p>
        </div>
      ))}
    </div>
  )
}
```

## まとめ

最後までご覧頂きありがとうございました。
