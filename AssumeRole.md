---
tags:
  - AWS
  - AWS/IAM
aliases:
  - スイッチロール
  - SwitchRole
---
## what
- [[プリンシパル]]に一時的に[[IAMロール]]を付与する仕組み
- コンソール上で「ロールの切り替え」をすることがSwitchRole。内部的にはAssumeRole APIを使用している。CLIやSDKではAssumeRoleと呼び、コンソール上ではSwitchRoleと呼ぶ感じ。

1. AssumeRoleしたいIAMユーザーのポリシーで`sts:AssumeRole`を許可する（[[アイデンティティベースのポリシー]]）
	```json
	{
		"Version": "2012-10-17",
		"Statement": [
			{
				"Effect": "Allow",
				"Action": "sts:AssumeRole",
				"Resource": "arn:aws:iam::123456789:role/dev-role"
			}
		]
	}
	```
2. [[IAMロール]]の[[信頼ポリシー]]でAssumeRoleできるプリンシパルを設定する
	```json
	{
	  "Version": "2012-10-17",
	  "Statement": [
	    {
	      "Effect": "Allow",
	      "Principal": {
	        "AWS": "arn:aws:iam::123456789:user/dev-user"
	      },
	      "Action": "sts:AssumeRole"
	    }
	  ]
	}
	```
3. [[AWS STS]]から一時認証情報を受け取る
	`$ aws sts assume-role --role-arn arn:aws:iam::123456789:role/dev-role --role-session-name dev-session --profile dev-user`
4. 一時認証情報を使って操作する
## how
### ターミナルアプリからassume roleする方法
```bash
# 一時的なクレデンシャルを要求
$ aws sts assume-role \
  --role-arn arn:aws:iam::xxxxxxxxxxxx:role/xxxxxxxxxxxx \ # スイッチ先のIAM RoleのARN 
  --role-session-name foo-bar-session \ # セッション名 自由に指定できる 
  --duration-second 900 \ 一時クレデンシャルの有効期限 900秒以上 
  --profile foo-bar # assume-role をコールするユーザ情報
# レスポンスのアクセスキーID, シークレットキー, セッショントークンを環境変数に格納すると、それ以降のawsコマンド実行はスイッチ先のアカウントに実行される
```
---
[イラストで理解するAssumeRoleの疑問](https://zenn.dev/fdnsy/articles/e98c43d9c3f611)