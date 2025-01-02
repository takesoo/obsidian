---
tags:
  - TypeScript
---
- [[extends]]で条件分岐を使った型
```ts
type IsNumber<T> = T extends number ? true : false;
```
## Distributive Conditional Types
 - Tにユニオン型が渡ってきた時のConditional Typeのこと
```ts
type CondigionalType<T, U> = T extends U ? X : Y;
```