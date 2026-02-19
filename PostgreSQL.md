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

## ベストプラクティス
- 原則
	1. 最小権限の原則を[[Role-Based-Access Controll|RBAC]]と[[Row-Level Security|RLS]]に適用する
	2. 権限は直接ユーザーに付与せず、グループを介して付与する
	3. RLSはシンプルかつ高速に保つ
		- `user_id`や`tenant_id`には必ずインデックスを貼る
		- `current_setting('app.current_user_id')`
	4. マルチテナントでは`tenant_id`をキーにする
	5. 管理者ロールには`ALTER ROLE admin_user BYPASSRLS;`を設定して、RLSをバイパスする
- 構成例
	- owner_role: 
		- BYPASSRLS
		- テーブル定義変更のみ
	- app_role:
		- RLS適用
		- レコード変更のみ
	- RLSポリシー: app_roleに対してのみ、セッション変数に基づいたフィルタをかける