---
tags:
  - database
  - postgresql
---
## Role
PostgreSQLでは、データベース内のオブジェクト（テーブル、シーケンス、スキーマなど）に対するアクセス権限を管理するために、ロール（ユーザーやグループ）を使う。物理的にはユーザーとグループに区別がなく、すべてロールという概念で扱われる。ログイン権限が付与されてるロールはユーザーとして振る舞い、付与されてないロールはグループとして振る舞う。
```sql
CREATE ROLE taro LOGIN PASSWORD 'password'; -- ログイン権限が付与されているのでユーザー
CREATE ROLE developer_group; -- ログイン権限が付与されてないのでグループ
GRANT developer_group TO taro; -- taroをdeveloper_groupに追加し、権限が継承される
```
ロールには特定の権限が付与され、それに応じてデータベース内で実行できる操作が決まる([[Role-Based-Access Controll|RBAC]])
