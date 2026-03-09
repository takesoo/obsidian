---
aliases:
  - "@layer"
  - カスケードレイヤー
tags:
  - CSS
---
## what
- スタイルの優先順位をグループ化する仕組み
	- CSSはIDセレクタやクラスセレクタによる定義方法によってどのスタイルが適応されるかが決まるが、Layerを使うと層によって優先度を設定できる
- 後から定義されたレイヤーが優先される
## why
- 従来のCSSではカスケードという仕組みでスタイルの記述方法による優先度を決定していたが難易度が高かったためカオス（`!important`多用）になりがちという問題があった。
## how
```css
/* global.css */

/* 1. Baseレイヤー: タグのデフォルトを定義 */
@layer base {
  body {
    background-color: #ffffff;
    text-color: #1f1f1f;
  }
  h1 {
    font-size: 18rem;
    font-waite: bold;
  }
}

/* 2. Componentsレイヤー: 複雑なコンポーネント */
@layer components {
  .btn-primary {
    pading-x: 4rem;
    pading-y: 2rem;
    boarder-radius: 2rem;
    background-color: #1f1f1f;
    font-color:hover: 80;
  }
}

/* 3. Utilitiesレイヤー: 独自の便利クラスを追加したい場合 */
@layer utilities {
  .text-shadow-sm {
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
  }
}
```