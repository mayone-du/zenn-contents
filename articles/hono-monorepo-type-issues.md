---
title: "Honoをmonorepoで使った際に起こった奇妙な型エラーの解決"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに

結論、tsconfig.json で`"composite": true`を設定して、`"references": [{ "path": "../other-lib" }]`を追加することで解決
