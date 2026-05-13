---
aliases:
  - .npmrc
---
## what
- [[npm]]( [[yarn]], [[pnpm]] )の設定ファイル
## how
- `engine-strict`: [[package.json]]の`engines`で指定したnodeおよびnpmのバージョンと異なるときにインストールを中止する。
- `save-exact`: パッケージ追加時にバージョン表記からキャレットを除去してバージョン固定にする