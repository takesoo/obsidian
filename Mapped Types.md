---
tags:
  - TypeScript
aliases:
  - in
overview: ユニオン型からオブジェクト型を作成する
---
```ts
type Keys = 'x' | 'y';
type Flags = { [K in Keys]: boolean }; // { x: boolean, y: boolean }

const flags: Flags = {
	x: true,
	y: false,
};

/**
 * プロパティの追加はできない
 */
 type Flags2 = {
	 [K in Keys]: boolean;
	 z: boolean; // A mapped type may not declare properties or method.
 };
 // インターセクション型にする必要がある
 type Flags3 = Flags & { z: boolean; };
```