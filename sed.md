---
tags:
  - Linux/コマンド
---
==S==tream ==Ed==itor
`sed スクリプトコマンド ファイル名`
指定したファイルをコマンドに従って処理して、標準出力に出力する
```bash
$ echo 'Hello, World!' | sed 's/Hello/Goodbye'
Goodbye, World!

-e 複数処理
$ echo 'Hello, World!' | sed -e 's/Hello/Goodbye' -e 's/World/Japan'
Goodbye, Japan!

-d 行を削除

```
