---
aliases:
  - Structured Query Language
tags:
  - データベース
---
データベースを操作する際に[[DBMS]]に出す命令文。
DDLとDMLの2つの言語で構成されている。
## DDL
Data Definition Language
データを定義する言語
- CREATE
- DROP
- ALTER
- TRUNCATE
## DML
Data Manipulation Language
データを操作する言語
- SELECT
- UPDATE
- DELETE
- INSERT 

[[SQLの実行順序]]に注意する。

[[OVER]]
PARTITION BY
GROUP BY
DISTINCT
WINDOW関数
EXIST
HAVING

SQLが遅くなる要因
join
DISTINCT
サブクエリの多様
HAVINGの過剰利用
order by

月毎にグループする
```sql
select DATE_FORMAT(`column`, '%Y-%m') as month, count(`column`) as count
from 'tests'
group by grouping_column;
```