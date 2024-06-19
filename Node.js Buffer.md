---
aliases:
  - Buffer
tags:
  - nodejs
---
固定長のバイト列を表現するために使用されるオブジェクト
Node.jsで[[2進数|バイナリデータ]]を扱う時に使用するクラス

```javascript
// 10バイトのメモリ領域を確保する(各バイトは0で初期化される)
Buffer.alloc(10);
// <Buffer 00 00 00 00 00 00 00 00 00 00>

// バイトの配列から生成
// 値の範囲は0~255
Buffer.from([1, 255, 256]);
// <Buffer 01 ff 00>

// 文字列から生成
Buffer.from('テスト');
// <Buffer e3 83 86 e3 82 b9 e3 83 88>

```