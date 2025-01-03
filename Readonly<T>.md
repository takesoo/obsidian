---
tags:
  - TypeScript/UtilityType
overview: オブジェクト型Tのプロパティすべてをreadonlyにする
---
```ts
type Person = {
  surname: string;
  middleName?: string;
  givenName: string;
};
type ReadonlyPerson = Readonly<Person>;
/**
 * type ReadonlyPerson = {
 *   readonly surname: string;
 *   readonly middleName?: string;
 *   readonly givenName: string;
 * };
 */
```