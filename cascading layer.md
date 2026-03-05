---
aliases:
  - "@layer"
tags:
  - CSS
---
## what
- スタイルの優先順位をグループ化する仕組み
	- CSSは基本的に後に書いたものが優先されるが、Layerを使うと層ごとに優先度を設定できる
- 基本の3層構造
	- `base`: 優先度低。リセットCSSや`h1`, `p`, `body`などの素のタグへのスタイル。
	- `components`: 優先度中。`.btn`, `.card`など、再利用するパーツのスタイル
	- `utilities`: 優先度高。`.m-4`, `.text-center`など、一つの役割を持つクラスのスタイル。
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