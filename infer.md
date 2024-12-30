---
tags:
  - TypeScript
overview:
---
- `extends`の右辺にのみ書くことができる
```ts
// ある関数の戻り値の型を返すユーティリティ型
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R
	? R
	: any
```