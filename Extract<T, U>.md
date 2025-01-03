---
tags:
  - TypeScript/UtilityType
overview: ユニオン型TからUで指定した型だけを抽出した型を返す
---
```ts
type Grade = "A" | "B" | "C" | "D" | "E";
type FailGrade = Extract<Grade, "D" | "E">; // "D" | "E";

// ユニオン型の共通部分を導き出す時にも使える
type CommonTypes = Extract<"a" | "b" | "c", "b" | "c" | "d">; // "b" | "c";
```
