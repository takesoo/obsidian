---
tags:
  - JavaScript
---
ブラウザ環境で[[2進数|バイナリデータ]]を扱う時に使用するクラス
バイナリデータを格納することができる[[バッファ]]
バイナリデータの内容を直接操作するための手段は提供していない。

```javascript
new ArrayBuffer(10);
// ArrayBuffer {
//   [Uint8Contents]: <00 00 00 00 00 00 00 00 00 00>,
//   byteLength: 10
// }

```