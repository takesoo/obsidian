---
tags:
  - mysql
aliases:
  - charset
  - 文字セット
---
- 記号とエンコーディングのセット
	- `utf8mb4`: UTF-8で文字ごとに1~4バイト使用する
	- `utf8mb3`: UTF-8で文字ごとに1~3バイト使用する
	- `utf8`: `utf8mb3`のエイリアス
- 各文字セットにはデフォルト[[mysql-collation|照合順序]]がある
	- `utf8mb4`なら`utf8mb4_0900_ai_ci`(MySQL8.0以降)
## クライアント側、サーバー側、それらの接続の文字コード
### `character_set_client`
クライアント側で発行したsql文はこの文字コードになる
### `character_set_connection`
クライアントから受け取った文字をこの文字コードへ変換する
### `character_set_resuilts`
クライアントへ送信する検索結果はこの文字コードになる
### `character_set_system`
システムの使用する文字セット
## データベース、テーブル、カラムの文字コード
### `character_set_server`
DB作成時のデフォルト文字コード
### `character_set_database`
DBの文字コード
