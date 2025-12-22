---
aliases:
  - CSS変数
---
## what
- CSSファイル内の特定のスコープ内で再利用可能な特定の値を表すエンティティ
- 値の定義を一箇所でし、複数箇所から参照できる
## why
- 複雑なWebサイトではCSSの値が重複することがあるため、値を一箇所で定義して他の複数箇所から参照できると変更容易性が増す
## how
### `--`prefixによる宣言
```css
:root {  /* 変数のスコープ。:rootはグローバル。 */
	--main-bg-color: brown;
}

details {
	background-color: var(--main-bg-color); /* brown */
}
```

### `@property`による宣言
```css
@property --box-color {
	syntax: "<color>"; /* --box-colorは<color>型を期待する */
	inherits: false; /* 継承を無効化 */
	initial-value: cornflowerblue; /* 初期値 */
}

.parent {
	--box-color: green; /* --box-colorにgreenを代入 */
	background-color: var(--box-color); /* green */
}

.child {
	background-color: var(--box-color); /* cornflowerblue */
}
```
