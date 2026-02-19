---
aliases:
  - RBAC
tags:
  - postgresql
---
## what
- ロール（ユーザーとグループ）に対して、「テーブルの読み取り」「データの挿入」などの権限を割り当てる
- `SELECT`, `INSERT`, `UPDATE`, `DELETE` などのアクション単位で設定
## why
- 「開発者」「経理」「閲覧専用」といった役割ごとに、どのテーブルに対してどのような操作を許可するかを設定できるようにするため
## how
```sql
CREATE ROLE readonly_group;
GRANT SELECT ON users TO readonly_group;
GRANT readonly_group TO taro;
```
