---
tags:
  - AWS
---
## what
- AWS認証情報の管理、ローテーションやコンソールログインコマンドの提供
## why
- `~/.aws/credentions`に平文で保存はリスキー
- ローテーション作業の手間を削減
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