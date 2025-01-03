---
tags:
  - TypeScript/UtilityType
overview: 関数型Tの戻り値を返す
---
```ts
type ReturnType1 = ReturnType<() => string>; // string;
type ReturnType2 = ReturnType<(arg: string) => string | number>; // string | number;
type ReturnType3 = ReturnType<() => never>; // never;

// typeofと併用する場合
const isEven = (num: number) => {
  return num / 2 === 0;
};
 
type isEvenRetType = ReturnType<typeof isEven>; // boolean;
```