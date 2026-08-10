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

### Role
| Postgres Role   | どういう状態？               | 主な用途      |             |
| --------------- | --------------------- | --------- | ----------- |
| `anon`          | ログインしていない             | 公開アクセス    |             |
| `authenticated` | Supabase Auth でログイン済み | 一般ユーザー    |             |
| `service_role`  | バックエンドの特権アクセス         | サーバー・管理処理 | `BYPASSRLS` |
### Supabase Data API利用の場合
```
		              Client
		                 │
		                 ▼
                  Supabase Data API
                         │
         ┌───────────────┼────────────────┐
         │               │                │
Publishable       Publishable + JWT     Secret
         │               │                │
         ▼               ▼                ▼
       anon        authenticated      service_role
         │               │                │
         └───────────────┼────────────────┘
                         ▼
                       GRANT
                         ▼
                        RLS
                         ▼
                    PostgreSQL
```
- Supabase Data API に対してアクセスするときに、付与しているキーと JWT トークンの有無によって適用されるロールが切り替わる。
- どのロールの場合にデータテーブルへのアクセスを許可するかどうかは、データベース側の`GRANT`によって決まる。
- Secret/service_roleの場合には、`BYPASSRLS` 権限が付与される。
### PostgreSQL直接接続の場合
```
DB Client(eg: drizzle...)
 ↓ 接続情報
postgres driver
 ↓
PostgreSQL
```
- 各種 API キーは基本的に使用しない。
- どのロールを使用するかは、接続情報から指定する。