---
tags:
  - AWS
---
## what
- aws cliのプロファイル管理を容易にかつセキュアにしてくれる
- 個人や小規模の場合にマッチ。大規模の場合は[[AWS IAM Identity Center]]がマッチ。
## why
- `~/.aws/credentions`に平文で保存はリスキー。OSのキーチェーン（macOS KeychainやWindows Credential Managerなど）に認証情報を暗号化して保存するため、設定ファイルにアクセスキーを直書きせずに済みます。
- 実行時にのみ一時的な環境変数を生成してコマンドを叩くため、キーの漏洩リスクが大幅に下がります。
## how
```shell
# アクセスキー追加
$ aws-vault add taro

$ aws-vault add takuzo # 名前は管理用なのでiam userと同じでなくて良い 
Enter Access Key ID: xxx 
Enter Secret Access Key: **************************************** 
## ここでキーチェーンが起動しaws-vault takuzo用のpasswordを入力 
Added credentials to profile "takuzo" in vault 
# 追加を確認 
$ aws-vault ls 
Profile  Credentials  Sessions 
=======  ===========  ======== 
takuzo   takuzo       - 
# 補足：作成されるのはconfigのみでcredentialsが作成されていない 

$ ls ~/.aws/
config

# 管理者ユーザーでterraformで構築
$ aws-vault exec admin -- terraform apply

# 開発ユーザーでawsコマンド実行
$ aws-vault exec dev-user -- aws s3 ls

# アクセスキーのローテーション
$ aws-vault rotate dev-user --no-session

# AWSコンソールへのログイン
$ aws-vault login dev-user
```

### 仕組み
`aws-vault exec <profile>`を実行すると環境変数に認証情報が出力される。この状態ではこの認証情報を使ってawsへの操作を行う。
```shell
$ aws-vault exec takuzo -- env | grep AWS
AWS_VAULT=takuzo
AWS_ACCESS_KEY_ID=ASIAxxxxxx
AWS_SECRET_ACCESS_KEY=xxx
AWS_SESSION_TOKEN=xxx
AWS_SECURITY_TOKEN=xxx
AWS_SESSION_EXPIRATION=2023-12-12T20:52:12Z
# aws-vaultなしでは何も表示されない
$ env | grep AWS
$ 

```