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

- イベント: Lambdaの実行を起動するトリガーとなるデータや出来事
	- API Gateway, S3, DynamoDB, CloudWatch Events, SQS, SNS
- ハンドラー関数: Lambdaが実行する際のエントリーポイントとなる関数
```python
def lambda_handler(event, context):
    # event: イベントデータ
    # context: 実行環境の情報
    return {
        'statusCode': 200,
        'body': 'Hello World'
    }
```
- Execution Role: Lambdaが他のAWSサービスにアクセスするための[[IAMロール]]
- Layer: 複数の関数で共有できるライブラリやコードのパッケージ
### Lambda実行環境
[Lambda 実行環境 - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/lambda-runtime-environment.html)
![[Pasted image 20250910093611.png]]
1. Initフェーズ
	- Lambda関数の初期化（拡張機能の起動、ランタイムのブートストラップ、静的コードの実行）
2. Restoreフェーズ(SnapStartのみ)
3. Invokeフェーズ
	- 関数が呼び出され、指定のタイムアウト内で関数本体と拡張機能が実行される
4. Shutdownフェーズ
	- 実行環境終了時に拡張機能へ通知され、クリーンアップ処理が行われる