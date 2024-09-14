---
tags:
  - AWS/Lambda
aliases:
  - Lambda
---
- サーバレスで関数を実行するサービス
- ソースコードのデプロイはZIP形式かDocker Image形式

---
- Docker Imageでアップロードする場合
	- docker build
	- docker push ECR_REPO_URI:latest
	- lambdaで↑のイメージを使用するように定義

## Lambda実行環境
[Lambda 実行環境 - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/lambda-runtime-environment.html)
1. Lambda ServiceはExecution Environmentを作成する
2. Execution Environment上でruntimeなどの初期化をする
3. その後、runtimeはRuntime APIを介してLambda Serviceと通信し、event, contextを取得。Function(Handler)にそれらを渡す
4. Functionの戻り値をRuntime API経由でLambda Serviceに渡す
5. Invokeがされなくなるまで3~4を繰り返す
6. 一定時間の間invokeされなくなったら、Lambda Serviceはruntimeをシャットダウンする