---
title: "GraphQL APIをfetchメソッドで叩く方法"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [graphql, javascript, typescript]
published: false
---

Chrome 拡張機能を作る際に GraphQL API を fetch メソッドで叩いたことがあったので、その際のメモになります。

↓ 例

```tsx:sample.tsx
const handleClick = () => {
  try {
    fetch("https://架空のGraphQLAPIエンドポイント.com/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        query: `
          mutation CreateSample(
            $title: String!
          ) {
            createSample(input: {title: $title}) {
              sample {
                title
              }
            }
          }
        `,
        variables: {title: inputTitle},
      }),
    })
      .then((response) => {
        console.log(response.json());
        return response.json();
      })
  } catch (error) {
    alert(error);
  }
};
```

適宜 mutation の部分を query にかえたり、非同期の処理周りを調整して下さい。
