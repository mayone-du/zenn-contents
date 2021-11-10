---
title: "爆速且つ超簡単におしゃんなターミナルを構築する"
emoji: "🐚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [mac, shell, terminal, starship, iterm2]
published: true
---

## はじめに

タイトル通り、一瞬でおしゃんな（使いやすい）ターミナルを構築します。

### 前提

- MacOS
- Homebrew の環境構築

### 今回やること

↓ のようなターミナルの構築(GIF は VSCODE のものですが、ターミナルも同じです)
![](/images/terminal/starship-demo.gif)

#### インストールするもの

https://starship.rs/
https://iterm2.com/
https://github.com/zsh-users/zsh-autosuggestions
https://github.com/yuru7/HackGen

## セットアップ

1. iTerm2 のインストール

```bash:terminal
brew install iterm2 --cask
```

2. hackgen-nerd-fonts のインストール

:::message
ここに関しては任意ですが、後にインストールする starship ではこのフォントがないと一部文字化けが合った気がするので、できれば入れてください。
フォント自体めちゃくちゃ見やすくておすすめなので、入れて損はないです 👍
:::

```bash:terminal
brew tap homebrew/cask-fonts
brew install hackgen-nerd-fonts
```

3. starship のインストール

```bash:terminal
brew install starship
```

4. zsh-autosuggestions のインストール

```bash:terminal
brew install zsh-autosuggestions
```

これをいれると、自分の過去のコマンドをサジェストしてくれてめちゃくちゃ便利です。
ただ、自分の履歴を表示してるだけなので、間違ったコマンドとかも覚えちゃうのでそれだけ注意が必要です。

5. iTerm2 の設定

文字や背景色、フォントなどをお好みで設定して下さい。
ちなみにですが、VSCODE のフォントも**HackGen35Nerd Console**にしておくと見やすいのでおすすめです！

6. .zshrc の設定

~/.zshrc に以下を追加

```shell:.zshrc
eval "$(starship init zsh)"
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

7. ターミナル、エディターを再起動

再起動すると、設定が反映されます。

## まとめ

最後までご覧頂きありがとうございました。
過去にやったことを思い出しながら書いてるので、もし間違っている箇所などあればご指摘下さい。
