secrets
- `credentials.yml.enc`と`ENV`を使い分ける

| 種類                                      | 保存場所            | 理由・補足                               |
| --------------------------------------- | --------------- | ----------------------------------- |
| Railsアプリ内で直接使うAPIキー（Stripe/AWS/Slackなど） | ✅ `credentials` | アプリの一部として密接に使う。安全性とGit管理が両立できる。     |
| 外部ミドルウェアの接続情報（DB/Redisなど）               | ✅ `ENV`         | インフラ側で管理するのが一般的。環境に依存しやすいため。        |
| Railsの `secret_key_base`                | ✅ `credentials` | Rails標準でここに保存される。暗号化トークンの元になる重要キー。  |
| 環境別に異なるWebホスト・S3バケット名など                 | ✅ `credentials` | `Rails.env.production?` などで分岐して使える。 |
| CI/CDの中でのみ使うトークン・キー                     | ✅ `ENV`         | 実行環境がアプリ外なので、credentialsに入れても使えない。  |
credentials
- Railsアプリケーションが扱う機密情報を保持するファイル（secret_key_base, サードパーティAPIのAPIキーなど）
- master.keyによって暗号化される
- 暗号化されるのでgit管理ができる
- `EDITOR="vim" bin/rails credentials:edit --environment production`
```bash
# credentialの変更
EDITOR="vim" bin/rails credentials:edit

# credentialの確認
EDITOR="vim" bin/rails credentials:show

# 環境ごとのcredentialの作成
# config/credentials/production.yml.encとconfig/credentials/production.keyが作られる
EDITOR="vim" bin/rails credentials:edit --environment production
```

secret_key_base
- Rails内部で使用される秘密鍵
- session情報を[[Cookie]]に保存するときの暗号化に使用される
- 一時的なリンクトークンの生成