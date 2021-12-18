---
title: "Apollo Clientのキャッシュを完全に理解する"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [apollo, apolloclient, graphql, typescript, cache]
published: false
---

## はじめに

業務で Apollo Client を使用しており、キャッシュについて少し知見がたまったので、メモがてら学んだことを記していきます。
といってもまだまだわかりきっていない部分も多いので、ご指摘等いただければ嬉しいです。

:::message
細かい React の書き方や、記事の本質とずれる部分は省略するので、適当に脳内で補完してください。
:::

### やること

- キャッシュオブジェクトの構造や仕組みの理解
- キャッシュがどのように保存されるのかを知る
- キャッシュの更新

## 本文

```tsx:sample.tsx
// data: {
//   posts: {
//     edges: [
//       {
//         node: {
//           id: "1",
//           title: "タイトル",
//           body: "本文",
//         },
//       },
//     ],
//   },
// }
// ↑クエリの結果はこのようなものだと思ってください。
const SampleComponent: React.VFC = () => {
  const { data } = usePostsQuery(); // generateされたqueryのhook
  const [addPost, { loading }] = useAddPostMutation(); // generateされたmutationのhook
  return (
    <div>
    {/* 投稿一覧 */}
      {data.posts.edges.map((post) => (
        <div key={post.node.id}>
          <h1>{post.node.title}</h1>
          <p>{post.node.body}</p>
        </div>
      ))}

      <div>
        <button onClick={() => {addPost()}}>作成</button>
      </div>
    </div>
  )
}
```

```graphql:query/posts.graphql
# 引数等は省略します
query Posts {
  edges {
    node {
      id
      title
      body
    }
  }
}
```

```graphql:mutation/addPost.graphql
# 引数等は省略します
mutation AddPost {
  addPost {
    id
    title
    body
  }
}
```

## まとめ

最後までご覧頂きありがとうございました。
