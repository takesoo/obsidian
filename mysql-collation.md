---
tags:
  - mysql
aliases:
  - 照合順序
  - collation
---
- 文字の間の順序を定義する項目
- MySQL5.7までは`utf8mb4_general_ci`がデフォルトだったが、8.0からは`utf8mb4_0900_ai_ci`
	- `utf8mb4_general_ci`
		- 半角大文字と半角小文字は同じ文字として判断される
		- カタカナとひらがなを別文字として扱う
	- `utf8mb4_0900_ai_ci`
		- 大文字小文字が明示的に区別されず、アクセントも区別されない
	- > つまり、CREATE TABLE文に`DEFAULT CHARSET=utf8mb4`のみ記述すると、`collation_server`システム変数に設定されているサーバーのデフォルトcollationでもデータベースレベルのcollationでもない、MySQLのデフォルトcollationが設定されてしまうのです。CREATE TABLE文を実行するときは予期せぬトラブルを避けるため、`DEFAULT CHARSET=xx COLLATE=xx`を省略せずに記述するのが良いでしょう。
	  https://gihyo.jp/dev/serial/01/mysql-road-construction-news/0157
	-  
