---
aliases:
  - .zshrc
---
- インタラクティブシェル起動時に読み込まれる
	- ターミナルで新しいシェルを開くたび
- aliasや保管設定などシェル操作に関する設定を記述する
```zsh
# ~/.zshrc の例
alias ll='ls -la'
export EDITOR=nvim
autoload -Uz compinit && compinit

```