---
tags:
  - TypeScript/UtilityType
overview: Promiseの解決値の型Tを返す
---
```ts
type Awaited1 = Awaited<string>; // string
type Awaited2 = Awaited<Promise<string>>; // string
type Awaited3 = Awaited<Promise<Promise<string>>>; // string
```