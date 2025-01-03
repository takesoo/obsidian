---
tags:
  - TypeScript/UtilityType
overview: Tの型推論を防ぐ
---
```ts
function getIndexFromArray<T extends string>(
  elements: T[],
  item: NoInfer<T> // NoInferがないとT=Fluit | "peach"に推論されてしまう。
): number {
  return elements.findIndex((element) => element === item);
}
 
type Fruit = "grape" | "apple" | "banana";
const fruits: Fruit[] = ["grape", "apple", "banana"];
getIndexFromArray(fruits, "apple");
getIndexFromArray(fruits, "peach"); // Argument of type '"peach"' is not assignable to parameter of type 'Fruit'.
```