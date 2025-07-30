---
tags:
  - AWS
  - AWS/CLI
---
## what
- [[AWS]]を操作できる[[CLI]]

## how
- `~/.aws/config`
	- 設定情報（リージョン、出力形式など）を保存する
```
[default]
region = ap-northeast-1
output = json

[profile hoge]
source_profile = default
mfa_serial = arn:aws:iam::12345:mfa/name
role_arn = arn:aws:iam:12345:role/AdminRole
```
- `~/.aws/credentials`
	- アクセスキーIDとシークレットキーなどの認証情報を保存する
```
[default]
aws_access_key_id = AKIA*****************
aws_secret_access_key = *********************

[hoge]
aws_access_key_id = AKIA*****************
aws_secret_access_key = *********************
```
```zsh
$ aws s3 list --profile=hoge
```