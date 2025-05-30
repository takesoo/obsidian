---
tags:
  - TypeScript
overview: 型の部品を抽出する
---
## what
- 「推測する」
- [[Conditional Type]]の中で使われる型演算子
- `extends`の右辺にのみ書くことができる
```ts
// ある関数の戻り値の型を返すユーティリティ型
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R // 関数の戻り値型のRを推論
	? R // 抽出した戻り値の型を返す
	: never

// 配列型、タプル型から型を抽出する
type Tuple = [number, '1', 100]
type ArrayLast<T> = T extends [...infer _, infer Last] ? Last : never; // 末尾の要素をLastに抽出し、Lastを返す
type Test1 = ArrayLast<[1, 2, 3]> // 3
```