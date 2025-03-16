- OLAP(オンライン分散処理)分析に特化したデータベース
- 高速な処理を得意としているらしい
---
永続モードとインメモリモード
```shell
# 永続モードで起動
duckdb test.duckdb
v1.2.1 8e52ec4395
Enter ".help" for usage hints.
D .databases
test: test.duckdb

# インメモリモードで起動
duckdb
v1.2.1 8e52ec4395
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
D .databases
memory: NULL
D .open test.duckdb # .openで永続化するファイルを指定。永続モードに移行する
D .databases
test: test.duckdb
```

読み取り
```sql
D select * from 'test.csv';
D select * from read_csv('test.csv');
D select * from read_csv('test.csv', header = false);
D SELECT * FROM 'test.parquet';
D SELECT * FROM read_parquet('test.parquet');
D select * from 'test.json';
D SELECT * FROM read_json_auto('test.json');

# grob構文で複数ファイルを読み取り
D select * from 'test/*.parquet';
D select * read_parquet('dir/**/*.parquet');

# mysql
D install mysql; # 拡張機能をインストール
D load mysql;
D attach 'host=localhost user=root port=3306 database=database password=password' as mysqldb (TYPE mysql);
D use mysqldb;
D select * from members limit 5;
```
