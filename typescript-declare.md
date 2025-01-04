---
tags:
  - TypeScript/型定義ファイル
aliases:
  - declare
---
TypeScriptに変数、関数、クラスなどが「存在する」ことを伝える。（アンビエント宣言）（???）

```ts
function hello(name) {
  return "Hello, " + name;
}

hello("taro"); // Cannot find name 'hello'.

declare function hello(name: string): string;

hello("taro");
// => "Hello, taro"
```