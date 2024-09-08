---
tags:
  - npm
  - ORM
---
Node.jsを対象としたオープンソースの[[Object-Relational Mapping|ORM]]
[[型安全]]なデータベースアクセスが特徴
[[TypeScript]]との相性がいい

## 機能
- ORM
- Accelerate
	データベースキャッシュ
- Pulse
	データベースイベントのサブスクリプション

## 構成するモジュール
- Prisma Client
	クエリビルダー。
- Prisma Migrate
	マイグレーションシステム。schema.prismaに基づいて実行される
- Prisma Studio
	DBを操作するためのGUIツール

## 互換性
### データベース
- [[PostgreSQL]]
- [[MySQL]]
- [[SQLite]]
- [[MongoDB]]
- etc...
### フレームワーク・ライブラリ
- [[Next.js]]
- [[NestJS]]
- etc...

## 設定ファイル
```
// schema.prisma

generator client {
  provider = “prisma-client-js”
}
datasource db {
  provider = “mysql”
  url      = env(“DATABASE_URL”)
}
model books {
  id Int @id @default(autoincrement())
  title String
  author String
  overview String
  created_at DateTime @default(now())
  updated_at DateTime @default(now())
}

```


---
[Prisma を使った効率的なバックエンド開発ワークフロー](https://zenn.dev/optimisuke/articles/387b30c547ac54)