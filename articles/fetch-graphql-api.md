---
title: "GraphQL APIをfetchメソッドで叩く方法"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [graphql]
published: false
---

Chrome 拡張機能を作る際に GraphQL API を fetch メソッドで叩いたことが合ったので、その際のメモになります。

```tsx:sample.tsx
const handleClick = () => {
  try {
    fetch("https://something-grpahql-api.com/", {
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
