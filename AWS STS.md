---
tags:
  - AWS
  - AWS/STS
aliases:
  - AWS Security Token Service
---
## what
- AWSリソースへのアクセスに必要な一時的なセキュリティ認証情報を発行するサービス
	- アクセスキーIDと秘密鍵
	- セッショントークン
- 主な機能
	- [[AssumeRole]]
	- [[GetSessonToken]]
	- [[GetCallerIdentity]]
## why
- 長期的なアクセスキーを使うよりもセキュリティリスクを軽減できる
## how
```zsh
# AWSアカウントIDを取得
$ aws sts get-caller-identity --profile my-name --query "Account" --output text
```