---
tags:
  - nodejs
---
- node.jsの[[パッケージ管理システム|パッケージマネージャ]]
- [[npm]]よりインストールが速い
- npmより厳密にモジュールのバージョンを固定している
- npmと互換性があり、同じpackage.jsonを使用できる

```shell
npm install -g yarn

yarn init
# 初期化
# package.jsonの作成

yarn add package-name
# 新たにパッケージインストール
#
# --dev | -D
# devDependenciesに追記

yarn install
# yarn.lockをもとにパッケージインストール

yarn remove
# パッケージのアンインストール
```