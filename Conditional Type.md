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
 - この時、ユニオン型の要素それぞれについて判定される
```ts
type MyExclude<T, U> = T extends U ? never : T;
type Test = MyExclude<'a' | 'b' | 'c', 'a' | 'c'>
//        = ('a' extends 'a' | 'c' ? never : 'a')
//        | ('b' extends 'a' | 'c' ? never : 'b')
//        | ('c' extends 'a' | 'c' ? never : 'c')
// => 'b'
```