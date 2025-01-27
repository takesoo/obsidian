---
tags:
  - JavaScript
---
- [[JavaScript]]用のAPIドキュメントジェネレーター
- 呼び出し元をホバーするとコードヒントが表示されるようになる
- TypeDocなどと組み合わせてリファレンスドキュメントを生成できる（あんまりやらない？）
```js
/**
* @description print string // 説明
* @function                 // 対象を関数としてマークする
* @param {string} str 文字列 // 引数
* @return {void}            // 戻り値
*/
function print(str) {
	console.log(str);
}
```

```ts
/**
* @description print string
* @param str 文字列
* @return
*/
function print(str: string): void {
	console.log(str);
}

```