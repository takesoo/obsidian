---
tags:
  - nodejs
---
- node.jsの[[パッケージ管理システム|パッケージマネージャ]]
- [[npm]]よりインストールが速い
- npmより厳密にモジュールのバージョンを固定している
- npmと互換性があり、同じpackage.jsonを使用できる

```
// install
npm install -g yarn

// package.jsonの作成
yarn init

// yarnでパッケージインストール
yarn

// パッケージの追加
yarn add [パッケージ名]

// パッケージのアンインストール
yarn remove
```