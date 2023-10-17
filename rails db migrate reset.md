---
tags:
  - Ruby_on_Rails
aliases:
  - rails db:migrate:reset
---
内部的には
1. 全データベースの削除（rails db:drop）
2. データベースの作成（rails db:create）
3. マイグレーション（rails db:migrate）