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

サブクエリ
FROM句
あまり使い道はない
```sql
select s1.age, s1.age_count
from (
	select age, count(age) as age_count
	from students
	group by age
) as s1
```
WHERE句
```sql
# 14歳の生徒が受講するコース
select distinct courseId
from enrollments
where studentId in (
	select id
	from students
	where age = 14
);
```
SELECT文の列指定
```sql
select name, age, (
	select avg(age)
	from students
)
from students;
```