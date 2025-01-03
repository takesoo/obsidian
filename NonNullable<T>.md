---
tags:
  - TypeScript/UtilityType
overview: ユニオン型Tからnullとundefinedを取り除いたユニオン型を返す
---
```ts
type String1 = NonNullable<string>; // string;
type String2 = NonNullable<string | null>; // string;
type String3 = NonNullable<string | undefined>; // string;
type String4 = NonNullable<string | null | undefined>; // string;
type Never1 = NonNullable<null>; // never;
type Never2 = NonNullable<undefined>; // never;
```