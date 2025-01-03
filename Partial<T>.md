---
tags:
  - TypeScript/UtilityType
overview: オブジェクト型Tのプロパティすべてをoptionalにする
---
```ts
type Person = {
  surname: string;
  middleName?: string;
  givenName: string;
};
type PartialPerson = Partial<Person>;
/**
 * type PartialPerson = {
 *   surname?: string | undefined;
 *   middleName?: string | undefined;
 *   givenName?: string | undefined;
 * };
 */
```