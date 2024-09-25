---
tags:
  - TypeScript/UtilityType
---
object型TからKeysで指定したプロパティを除いたobject型を返す
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
以下と同じ
type Person = {
  surname: string;
  middleName?: string;
  givenName: string;
};
*/
```