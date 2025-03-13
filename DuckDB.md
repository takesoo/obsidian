- OLAP(オンライン分散処理)分析に特化したデータベース
- 高速な処理を得意としているらしい
---
読み込み
```sql
D select * from 'test.csv';
D select * from read_csv('test.csv');
D select * from read_csv('test.csv', header = false);
D SELECT * FROM 'test.parquet';
D SELECT * FROM read_parquet('test.parquet');
D select * from 'test.json';
D SELECT * FROM read_json_auto('test.json');

# mysql
D install mysql;
D attach 'host=localhost user=root port=3306 database=database password=password' as mysqldb (TYPE mysql);
D use mysqldb;
D select * from members limit 5;
```
