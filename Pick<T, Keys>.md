---
tags:
  - TypeScript/UtilityType
overview: 型TからKeysに指定したキーだけを含むオブジェクト型を返す
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
type Person = Pick<User, "surname" | "middleName" | "givenName">;
/**
 * type Person = {
 *   surname: string;
 *   middleName?: string;
 *   givenName: string;
 * };
 */
```