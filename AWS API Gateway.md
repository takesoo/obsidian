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
- エンドポイントタイプ
	- エッジ最適化：[[CloudFront]]経由でルーティングされる
	- リージョン：リージョンに直接ルーティングされる
	- プライベート：[[VPCエンドポイント]]経由でのアクセスのみ可能
- ステージ
	- prod, staging, dev
	- 過去のバージョンへの切り戻しが可能
### REST API
- `https://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>/...`
- 処理の流れ
	1. (クライアント)
	2. メソッドリクエスト
	3. 統合リクエスト
	4. (統合バックエンド)
	5. 統合レスポンス
	6. メソッドレスポンス

### WebSocket API
- WebSocket URL: `wss://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>`
- Callback URL: `https://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>/@connections/<connection-id>`
- WebSocketの状態に関するイベントを「ルート(Route)」として定義する
	- $connect, $disconnect, $defaultの3つのルートは事前定義されている
	- ルート選択式で評価するデータがない場合は$defaultが利用される
	- カスタムルートを定義し、メッセージデータ内でカスタムルートを指定すると、そのルートが選択される

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