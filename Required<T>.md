---
tags:
  - TypeScript/UtilityType
overview: オブジェクト型Tのすべてのプロパティを必須にする
---
```ts
type Person = {
  surname: string;
  middleName?: string;
  givenName: string;
};
type RequiredPerson = Required<Person>;
/**
 * type RequiredPerson ={
 *   surname: string;
 *   middleName: string;
 *   givenName: string;
 * }
 */
```