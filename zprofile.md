---
aliases:
  - .zprofile
---
- ログインシェル起動時に読み込まれる
	- macOSでは、ターミナル起動時
- [[PATH]]や[[LANG]]など環境変数の設定を記述する
```zsh
# ~/.zprofile の例
export PATH="$HOME/bin:$PATH"
export LANG=ja_JP.UTF-8

```