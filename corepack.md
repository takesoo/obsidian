---
tags:
  - nodejs
  - npm
---
https://github.com/nodejs/corepack?tab=readme-ov-file#utility-commands

node.jsのパッケージマネージャのバージョン管理ツール
グローバルにcorepackをインストールしていて、プロジェクトのpackage.jsonにパッケージマネージャーと利用バージョンを指定しておくと、そのパッケージマネージャーを自動的にインストールして使用できる。

```bash
// パッケージマネージャーとバージョンを指定
$ corepack use pnpm@8.15.5
```