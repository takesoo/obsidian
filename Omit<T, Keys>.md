---
tags:
  - TypeScript/UtilityType
overview: オブジェクト型TからKeysで指定したプロパティを除いたオブジェクト型を返す
---
```ts
type User = {
  surname: string;
  middleName?: string;
  givenName: string;
  age: number;
  address?: string;
  nationality: string;
  createdAt: string;
  updatedAt: string;
};
type Optional = "age" | "address" | "nationality" | "createdAt" | "updatedAt";
type Person = Omit<User, Optional>;
/*
 * type Person = {
 *   surname: string;
 *   middleName?: string;
 * };
 */
```