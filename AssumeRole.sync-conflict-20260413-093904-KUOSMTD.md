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

---
[イラストで理解するAssumeRoleの疑問](https://zenn.dev/fdnsy/articles/e98c43d9c3f611)