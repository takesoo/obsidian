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
- 「next-intlで多言語対応を実装して」とAIに雑に指示を出すと雑に実装されて動かないことが多々あるので、結局ライブラリについてちゃんと実装方法を学んだ上で詳細に指示を出す必要があった。
- next-auth.jsの使い方を理解するのに時間がかかった。トークンを取得した後の扱い方や、API叩いて