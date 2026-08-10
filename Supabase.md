## what
- [[PostgreSQL]]ベースのBaaS
## how

### キーと認証

| キー                    | 形式                     | 概要              | 認可範囲                         | 注意          |
| --------------------- | ---------------------- | --------------- | ---------------------------- | ----------- |
| publishable           | `sb_publishable_xxxxx` | 通常の認証キー         | Grant: 適用される<br>RLS: 適用される   |             |
| secret                | `sb_secret_xxxxx`      | RLSをバイパスする管理者キー | Grant: 適用される<br>RLS: バイパスされる | ブラウザへの公開はNG |
| anon (Legacy)         | 長期間有効JWT               | 通常の認証キー         | Grant: 適用される<br>RLS: 適用される   |             |
| service_roel (Legacy) | JWT                    | RLSをバイパスする管理者キー | Grant: 適用される<br>RLS: バイパスされる | ブラウザへの公開はNG |
- publishableキーは単なるAPIキー。Supabase AuthのJWTと一緒にAPIに送ることで、RLSが適用される。
```
Browser
  │
  │ Cookie
  │ └─ Supabase Auth のユーザーセッション
  ▼
Next.js
Route Handler
  │
  │ Publishable Key
  │ +
  │ ユーザーの Access Token (JWT)
  ▼
Supabase Data API
  │
  │ role = authenticated
  │ auth.uid() = ユーザーID
  ▼
RLS
  │
  ├─ tenant A のメンバー → tenant A のデータだけ
  └─ tenant B のメンバー → tenant B のデータだけ
  ▼
PostgreSQL
```
### Role
| Postgres Role   | どういう状態？               | 主な用途      |
| --------------- | --------------------- | --------- |
| `anon`          | ログインしていない             | 公開アクセス    |
| `authenticated` | Supabase Auth でログイン済み | 一般ユーザー    |
| `service_role`  | バックエンドの特権アクセス         | サーバー・管理処理 |
