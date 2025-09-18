---
tags:
  - AWS/Lambda
aliases:
  - Lambda
---
## what
- サーバレスで関数を実行するサービス
- ランタイムを抽象化したサービス
- ソースコードのデプロイはZIP形式かDocker Image形式
	- Docker Imageでアップロードする場合
		- docker build
		- docker push ECR_REPO_URI:latest
		- lambdaで↑のイメージを使用するように定義
- 内部的にはFirecrackerという技術をベースに稼働している。コンテナのようなもの。
- Lambda関数: AWS Lambdaで実行するアプリケーションそのもの
	- コードは依存関係も含めてビルド、パッケージングした上でS3に暗号化されて保存される
	- メモリ
		- 128MB~3008MB
		- 容量に応じてCPU能力なども比例する
	- タイムアウト
		- 最大900sec(15min)
	- 実行ロール: AWSリソースへのアクセスを許可する[[IAMロール]]
- ハンドラー関数: Lambdaが実行する際のエントリーポイントとなる関数
	- json形式のイベントデータにアクセスできる
```python
def lambda_handler(event, context):
    # event: イベントデータ
    # context: 実行環境の情報
    return {
        'statusCode': 200,
        'body': 'Hello World'
    }
```
- イベントソース: Lambdaの実行を起動するトリガーとなるデータや出来事
	- API Gateway, S3, DynamoDB, CloudWatch Events, SQS, SNS
	- ポーリングベース: Lambdaがポーリングして処理するデータがある場合に実行する
		- ストリームベース
			- [[DynamoDB]], [[AWS Kinesis]], etc
		- ストリーム以外
			- 
	- ポーリング以外: 呼び出し元のサービス側からLambdaを呼び出す
		- イベントソールマッピング：呼び出し元のサービス側で、呼び出すLambdaファンクションの設定情報を保持する
- Layer: 複数の関数で共有できるライブラリやコードのパッケージ
- Lambda実行環境([Lambda 実行環境 - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/lambda-runtime-environment.html))
![[Pasted image 20250910093611.png]]
	1. Initフェーズ
		- Lambda関数の初期化
			- 拡張機能の起動、ランタイムのブートストラップ、静的コード（ハンドラー関数外のコード）の実行
	2. Restoreフェーズ(SnapStartのみ)
	3. Invokeフェーズ
		- ハンドラー関数が呼び出され、指定のタイムアウト内で関数本体と拡張機能が実行される
	4. Shutdownフェーズ
		- 実行環境終了時に拡張機能へ通知され、クリーンアップ処理が行われる
- コールドスタート: Lambda関数を1から起動させること
- ウォームスタート: 起動済みのLambdaコンテナに対してInvokeフェーズだけ実行すること。
- Lambda Extensions: Lambda関数と並行して実行される軽量なプロセスで、Lambda関数のライフサイクル全体を通じて動作し、監視、セキュリティ、ガバナンス、その他の機能を提供する
- Lambdaエイリアス：特定のバージョンに対するエイリアス。devやprodなど
- 修飾ARN：バージョン番号やエイリアス名が末尾に付与されたLambda関数のARN。特定のバージョンを明確に指定して関数を呼び出すために使用される。
## how
## ベストプラクティス・アンチパターン
## セキュリティ