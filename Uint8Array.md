---
tags:
  - JavaScript
---
unsigned integer 8bit array
符号なし8ビット整数の配列
[[ArrayBuffer]]のデータを操作するためのインターフェースを提供するオブジェクト

```javascript
// 10バイトのArrayBufferを作成
let buffer = new ArrayBuffer(10);

// Uint8Arrayビューを作成し、bufferにバインド
let uint8View = new Uint8Array(buffer);

// ビューを通してデータを操作
uint8View[0] = 255;
uint8View[1] = 128;

console.log(uint8View[0]); // 255
console.log(uint8View[1]); // 128

```