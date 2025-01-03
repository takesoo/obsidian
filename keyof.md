---
tags:
  - TypeScript
overview: オブジェクトのキーをユニオン型にして返す
---
```ts
type Obj = {
	prop1: string;
	prop2: number;
};

type Keys = keyof Obj; // 'prop1' | 'prop2'
```