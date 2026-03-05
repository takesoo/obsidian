---
aliases:
  - tailwind
  - Tailwind
---
## what
- マークアップにutility classを設定するだけでスタイリングできるライブラリ
## why
- メリット
	- 素早く開発できる：クラス名に悩むこともなく、セレクタを決める必要もなく、HTMLとCSSファイルを切り替える必要もない。
	- 安全に変更できる：utility classの追加削除は対象の要素にのみ影響するため、他の要素への予期せぬ影響をなくすことができる。
	- 古いプロジェクトのメンテを楽に：変更を加える際は要素を探してクラスを変更するだけで済む
	- コードの移植性が高くなる：構造とスタイルが同じ場所に定義されるため、コピペしやすい
	- CSSの肥大化を止められる：utility classが再利用性に長けているので新たにCSSを書く必要がない
## how

### デザイントークンの設定方法
- `@theme`ブロックに記述する
- カラー、フォント、スペーシングなどの「定数」を定義する
- [[cascading layer]]も併用するのが基本
```css
@import "tailwindcss";

/* 1. テーマ定義 (変数の設定) */
@theme {
  --color-main: #1a1a1a;
}

/* 2. Baseレイヤー: タグのデフォルトを定義 */
@layer base {
  body {
    /* @apply: cssファイル内でtailwindユーティリティクラスを使用するための宣言。 */
    @apply bg-white text-gray-900 antialiased; /* bodyタグではbg-white text-gray-900 antialiasedのスタイルが当たるようになる */
  }
  h1 {
    @apply text-3xl font-bold;
  }
}

/* 3. Componentsレイヤー: 複雑なコンポーネント */
@layer components {
  .btn-primary {
    @apply px-4 py-2 rounded bg-[--color-main] text-white hover:opacity-80 transition;
  }
}

/* 4. Utilitiesレイヤー: 独自の便利クラスを追加したい場合 */
@layer utilities {
  .text-shadow-sm {
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
  }
}
```
```html
<div className="text-color-main">
	hoge
</div>
```

### container query
```html
<div class="@container">
  <div class="flex flex-col @md:flex-row">
    <!-- ... -->
  </div>
</div>
```