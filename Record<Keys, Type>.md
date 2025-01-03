---
tags:
  - TypeScript/UtilityType
overview: プロパティのキーがkeys、バリューがTypeであるオブジェクト型を作る
---
```ts
type StringNumber = Record<string, number>;
const value: StringNumber = { a: 1, b: 2, c: 3 };

type Person = Record<"firstName" | "middleName" | "lastName", string>;
const person: Person = {
	firstName: "Robert",
	middleName: "Cecil",
	lastName: "Martin",
};
```