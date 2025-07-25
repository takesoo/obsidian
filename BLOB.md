---
aliases:
  - Binary Large Object
tags:
  - データベース
---
## what
- ファイルのこと
	- 文書、画像、音声、動画、実行ファイル、圧縮ファイル、etc...
- 実体は大容量[[2進数|バイナリデータ]]
- データをファイルとして扱いたい時に使用する
```ts
// 例1: 文字列からBlobを作る
const text = "こんにちは";
const blob = new Blob([text], { type: 'text/plain' });

console.log(blob);
/**
 * 出力: Blob { 
 *   size: 15,          // データのサイズ（バイト数） 
 *   type: "text/plain" // データの種類
 * }
 */

```
