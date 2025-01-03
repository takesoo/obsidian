---
tags:
  - TypeScript/UtilityType
overview: ユニオン型TからUで指定した型を取り除いたユニオン型を返す
---
```ts
type Grade = "A" | "B" | "C" | "D" | "E";
type PassGrade = Exclude<Grade, "E">; // "A" | "B" | "C" | "D";
```