---
aliases:
  - nvm
---
## what
- [[Node.js]]のバージョン管理ツール
- [[npm]]のバージョン管理もできる
## why
- プロジェクトごとに異なるバージョンのNode.jsが必要になるため
## how
```shell
nvm install stable --latest-npm
nvm install --lts --latest-npm
nvm use stable
```

Node.jsのバージョンを固定したい場合は`.nvmrc`を使用する