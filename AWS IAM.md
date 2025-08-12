---
tags:
  - AWS/IAM
aliases:
  - Identity and Access Management
---
## what
- [[AWS入門ブログリレー2024〜AWS IAM編〜  DevelopersIO-2025-08-12 08-58-19]]
- 認証認可
- 「誰（何）が」「何に対して」「どのような操作を」「どの条件で」実行できるかを制御するサービス
- 最小権限の原則
## 構成要素
- [[AWS入門ブログリレー2024〜AWS IAM編〜  DevelopersIO-2025-08-12 08-58-19]]を参照。公式ドキュメントでプリンシパルとかIAM IDとかの言葉の正確な定義にこだわると沼にハマるので文脈からなんとなくで読み取ること。
### Policy
- 権限を定義するJSONドキュメント
- 定義方法による種類
	- AWS管理ポリシー：AWSが提供する定義済みポリシー
	- カスタマー管理ポリシー：ユーザーが独自に定義したポリシー
	- インラインポリシー：プリンシパルに直接埋め込まれる、ユーザーが独自に定義したポリシー。あまり使われることがないレガシー
- 付与する先による種類
	- アイデンティティベースのポリシー：
		- IAMユーザー、IAMグループ、IAMロールに関連づけるポリシー
		- 権限の付与にはAWS管理ポリシーを使い、制限するときにはカスタマー管理ポリシーで定義する運用がおすすめ
	- リソースベースのポリシー：AWSサービスに関連づけるポリシー
- 信頼ポリシー：[[AssumeRole]]設定のためのポリシー
```json
// アイデンティティベースポリシー
// 「bob」が「s3:::example-bucket」に対して「PutObjectとPutObjectAcl」することを「許可」する
{
	"Version": "2012-10-17",
	"Statement": {
		// 許可or拒否
		"Effect": "Allow",
		// どのAWSリソースが
		"Principal": {"AWS": "arn:aws:iam::777788889999:user/bob"},
		// どの権限を
		"Action": [
			"s3:PutObject",
			"s3:PutObjectAcl"
		],
		// どのリソースに対して
		"Resource": "arn:aws:s3:::example-bucket/*"
	}
}

// リソースベースポリシー
// 「arn:aws:logs:*:*:*」リソースに対して、「ログの作成権限とS3へのフルアクセス権限」を「許可」する
{
	"Version": "2012-10-17",
	// 権限の具体的な定義
	"Statement": [
		{
			// 許可or拒否
			"Effect": "Allow", // "Allow"|"Deny"
			// 実行可能な操作
			"Action": [
				"logs:CreateLogGroup",
				"logs:CreateLogStream",
				"logs:PutLogEvents",
				"s3:*"
			],
			// 操作対象のリソース
			"Resource": "arn:aws:logs:*:*:*"
		}
	]
}

```

## ベストプラクティス
- https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html
- IAM Identity Centerのユーザー、フェデレーティッドプリンシパルが推奨。サインイン時にIAMロールを引き受け、一時的な認証情報を使用するため。