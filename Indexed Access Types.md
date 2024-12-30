---
tags:
  - TypeScript
aliases:
  - インデックスアクセス型
overview: オブジェクトのプロパティ、配列の要素の型を参照する
---
```ts
/**
 * オブジェクト型とインデックスアクセス型
 */
type A = { foo: number };
type Foo = A["foo"]; // number

// ユニオン型によるインデックスアクセス型
type Person { name: string; age: number; };
type T = Person["name" | "age"]; // string | number
type T = Person[keyof Person]; // keyofを使用した書き方

/**
 * 配列型とインデックスアクセス型
 */
type MixedArray = (string | undefined)[];
type T = StringArray[number]; // number | undefined

// typeofとの組み合わせ
const array = [null, "a", "b"];
type T = (typeof array)[number]; // string | null

/**
 * タプル型とインデックスアクセス型
 */
type Tuple = [string, number];
type T = Tuple[0]; // string

// typeofとの組み合わせ
const stateList = ["open", "closed"] as const;
type State = (typeof stateList)[number]; // "open" | "close"
```