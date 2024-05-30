---
tags:
  - database
  - postgresql
---
## 分類
- 論理バックアップ
- 物理バックアップ
	- オフラインバックアップ
	- オンラインバックアップ


## 論理バックアップ
データをSQLで抽出する
```shell
// ダンプ
$ pg_dump postgres > dumpfile
// リストア
$ psgl restore < dumpfile
```
## 物理バックアップ
データファイル自体をコピーする
### オフラインバックアップ
PostgreSQLを停止した状態で、データファイルがあるディレクトリをコピーする
### オンラインバックアップ
PostgreSQLが起動した状態で、データファイルのコピーと[[WALアーカイブ]]を使用してバックアップする。




---
[PostgreSQLのバックアップ手法のまとめ #PostgreSQL - Qiita](https://qiita.com/bwtakacy/items/65260e29a25b5fbde835)