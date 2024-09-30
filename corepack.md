---
tags:
  - nodejs
  - npm
---
https://github.com/nodejs/corepack?tab=readme-ov-file#utility-commands

node.jsのパッケージマネージャ([[npm]], [[yarn]], [[pnpm]])のバージョン管理ツール
グローバルにcorepackをインストールしていて、プロジェクトのpackage.jsonにパッケージマネージャーと利用バージョンを指定しておくと、そのパッケージマネージャーを自動的にインストールして使用できる。

```bash
// パッケージマネージャーとバージョンを指定
$ corepack use pnpm@8.15.5
```

2024/09/30追記
[[Node.js]]にバンドルされなかったみたい。今後はあまり優位性がないかも。