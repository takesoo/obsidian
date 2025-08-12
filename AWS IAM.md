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
![[Pasted image 20240606185917.png]]
### Principal
- リクエストを行う主体
	- Root user
	- IAM user
	- IAM role
### IAM User
- コンソールにログインできる作業者アカウント
- 直接ポリシーをアタッチできるが、現在は一時的にロールを付与する運用（[[AssumeRole|SwitchRole]]）が好まれる
### IAM Group
- IAM Userの論理的な集合
- 例：運用、開発、管理
### IAM Role
- 一時的に引き受けることができる権限のセット
- IAM UserやAWSサービス、外部システムが一時的に特定の権限を取得する際に使用する
### ワークロード
- アプリケーション、プロセス、運用ツール、他のコンポーネントなど、ビジネス価値を提供するリソースとコードのコレクション
- AWS リソース？
### フェデレーティッドプリンシパル
- 外部のIDプロバイダーによって管理されているIAM User
### Policy
- 権限を定義するJSONドキュメント
- 定義方法による種類
	- AWS管理ポリシー：AWSが提供する定義済みポリシー
	- カスタマー管理ポリシー：ユーザーが独自に定義したポリシー
	- インラインポリシー：プリンシパルに直接埋め込まれる、ユーザーが独自に定義したポリシー
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
## 評価プロセス
1. 明示的なDeny
2. 明示的なAllow
3. 何も定義されていなければ拒否
![[Pasted image 20240606185944.png]]
## ベストプラクティス
- https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html
- IAM Identity Centerのユーザー、フェデレーティッドプリンシパルが推奨。サインイン時にIAMロールを引き受け、一時的な認証情報を使用するため。