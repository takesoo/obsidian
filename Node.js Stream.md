---
tags:
  - nodejs
---
JavaScriptでファイルを読み込むと、ファイル全体をメモリ上に乗せてオブジェクトに書き込む形になるのでメモリリークや遅くなる。streamはデータを小分けにして読み込むことでメモリ効率が良くなる。

Streamでは内部バッファ([[Node.js Buffer|Buffer]])にデータを格納する（バッファリング）

## streamの種類
### Readable
データの読み込み用Stream
内部バッファに少しずつデータを読み込む
内部バッファからデータを取り出す方法にはnon-flowingモードとflowingモードがある
non-flowing
	read()メソッドを明示的に呼び出して取り出す。
flowing
	Streamのdataイベントにリスナーをアタッチするとflowingモードに切り替わる。
	データが到着するとすぐにdataイベントが発火し、内部バッファのデータがリスナーに引き渡される。
### Writable
データの書き込み用Stream
write()メソッドを使用してStreamにデータをプッシュする。
### Duplex
読み込み可能かつ書き込み可能なStream
### Transform
読み書きされたデータの変更や変換

## pipe
Readable Streamの内部バッファからflowingモードでデータを取り出し、指定したWritable Streamに全てのデータをプッシュする。

## Event Emitter
各Streamの処理のタイミングに応じて発火するイベント
### Readable Streamのイベント
- readable
- data: 取り出されたデータがコンシューマに渡される時に発火する。リスナーがアタッチされるとflowingモードになる。
- pause
- resume
- end: ストリームから抽出できるデータがなくなったときに発火する
- close
- error: ストリーム内でエラーが発生したときに発火する
### Writable Streamのイベント
- drain
- pipe: pipe()メソッドがReadable Stream上で呼び出されたときに発火する
- unpipe
- finish: end()メソッドが呼び出され、処理が全て完了したときに発火する
- close: リソースが閉じたときに発火する
- error: ストリーム内でエラーが発生したときに発火する

---
[Node.js Stream を使ってみる｜ラキール公式｜株式会社ラキールのエンジニアたちによるTECH BLOG](https://tech-blog.lakeel.com/n/n62073e6f3101)