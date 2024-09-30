---
tags:
  - npm
  - バージョン管理ツール
---
[[Node.js]]のバージョン管理ツール
```shell
# make cache folder (if missing) and take ownership
$ sudo mkdir -p /usr/local/n
$ sudo chown -R $(whoami) /usr/local/n

# LTSバージョン確認
$ n --lts
14.16.0

# LTSバージョンをインストール
$ n lts

# latestインストール
$ n latest

# インストール可能なバージョンを表示
$ n lsr

# バージョン指定してインストール
$ n 15.10.0

# バージョン切り替え
# カーソル移動で選択
$ n
  o node/14.16.0
    node/15.10.0
```