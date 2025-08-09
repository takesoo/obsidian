---
tags:
  - AWS/ECS
aliases:
  - ECS
---
## ecs task role
- タスク（コンテナ内のアプリケーション）がAWSリソースへアクセスする権限
- アプリがS3バケットからファイルを読み書きする場合、このロールにS3アクセス権限を付与する
## ecs task execution role
- タスク起動時に必要な権限
- 例
	- ECRからイメージを取得
	- CloudWatch Logsにログ送信
	- SecretManagerから環境変数を取得