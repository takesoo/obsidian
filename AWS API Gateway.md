---
tags:
  - AWS
---
## what
- フルマネージドなAPIの窓口
	- 自動スケール
	- OS管理不要
	- 使用量に応じた課金
- REST APIやWebSocket APIを作成、公開、維持、監視、保護できる
- リージョンレベルで動作する
- 
### REST API

### WebSocket API

### 
### Lambdaプロキシ統合
- API Gatewayと[[AWS Lambda]]を連携させる統合方式の一つで、最も一般的で推奨される方法
- API Gatewayが受け取ったHTTPリクエストのすべての情報（ヘッダー、クエリパラメータ、パスパラメータ、ボディなど）を、JSONオブジェクトとしてLambda関数に渡す
- Lambda関数は定められた形式でレスポンスを返す必要がある

### ステージ変数
- デプロイメントステージごとに異なる設定値を持つことができる環境固有の変数
```
dev ステージ:
  - dbEndpoint = dev-database.example.com
  - lambdaVersion = v1
  - logLevel = DEBUG

prod ステージ:
  - dbEndpoint = prod-database.example.com  
  - lambdaVersion = v2
  - logLevel = ERROR
```
- 活用例
	- キックするLambda関数の切り替え
	```
	統合設定: arn:aws:lambda:region:account:function:myFunction:${stageVariables.LambdaAlias}

dev: LambdaAlias = dev
	prod: LambdaAlias = prod
	```

## 