---
title: "Reactの便利な型"
emoji: "🤟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react, nextjs, typescript, frontend]
published: false
---

## React の便利な型

ComponentProps<T>

ComponentProps<typeof Button>['onClick']

```tsx:Sample.tsx
type Props = Readonly<{
  onClick: VoidFunction
}>

const Sample: FC<Props> = ({ onClick }) => {
  return (
    <div>
      Sample
    </div>
  )
}

```
