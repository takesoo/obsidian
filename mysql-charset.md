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
- 