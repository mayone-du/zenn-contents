---
title: "爆速且つ超簡単におしゃんなターミナルを構築する"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [mac]
published: false
---

## はじめに

タイトル通り、一瞬でおしゃん（使いやすい）ターミナルを構築します。

### 前提

- MacOS
- Homebrew の環境構築

### 今回やること

## 本文

1. iTerm2 のインストール

```bash:terminal
brew cask install iterm2
```

2. iTerm2 の設定
3. starship のインストール

```bash:terminal
brew install starship
```

4. zsh-autosuggestions のインストール

```bash:terminal
brew install zsh-autosuggestions
```

5. hackgen-nerd-fonts のインストール

```bash:terminal
brew tap homebrew/cask-fonts
brew cask install hackgen-nerd-fonts
```

6. .zshrc の設定

```zshrc:.zshrc
eval "$(starship init zsh)"
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

## まとめ

最後までご覧頂きありがとうございました。
