---
tags:
  - AWS/IAM
---
AWSリソースへのアクセス制御をまとめたもの

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			// 許可or拒否
			"Effect": "Allow", // "Allow"|"Deny"
			// どの権限に対して
			"Action": [
				"logs:CreateLogGroup",
				"logs:CreateLogStream",
				"logs:PutLogEvents",
				"s3:*"
			],
			// どのリソースに対して
			"Resource": "arn:aws:logs:*:*:*"
		}
	]
}
```