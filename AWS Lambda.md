---
tags:
  - AWS/Lambda
aliases:
  - Lambda
---
## what
- サーバレスで関数を実行するサービス
- ソースコードのデプロイはZIP形式かDocker Image形式

---
- Docker Imageでアップロードする場合
	- docker build
	- docker push ECR_REPO_URI:latest
	- lambdaで↑のイメージを使用するように定義

### Lambda実行環境
[Lambda 実行環境 - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/lambda-runtime-environment.html)
![[Pasted image 20250910093611.png]]
1. Initフェーズ
	- Lambda関数の初期化（拡張機能の起動、ランタイムのブートストラップ、静的コードの実行）
	- 
2. Restoreフェーズ(SnapStartのみ)
3. Invokeフェーズ
4. Shutdownフェーズ