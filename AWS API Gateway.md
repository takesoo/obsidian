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


|      | REST API                                                                       | WebSocket API                                                                                                                                                                 |
| ---- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url  | `https://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>/...`      | `wss://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>`<br><br>`https://<api-id>.execute-api.<region-id>.amazonaws.com/<stage-name>/@connections/<connection-id>` |
| 認証認可 | エンドポイント単位で設定<br>- [[Amazon Cognito]]<br>- Lambda オーソライザー<br>- IAM認証<br>- APIキー | API単位で設定<br>- Lambda オーソライザー<br>- IAM認証                                                                                                                                       |

### 認証認可
- RESTの場合はエンドポイント単位で認証設定が可能
- WebSocketの場合はAPI単位での認証設定
- 認証方式
	- IAMアクセス権限
	- Lambdaおーそライザー：独自認証
	- Cognitoおーそライザー：Cognitoユーザープールをもとに事前に認証。WebSocketでは利用できない
### 統合リクエスト・統合レスポンス
- 統合タイプ：APIのバックエンドが何かを設定するもの
	- Lambda関数
	- HTTP
	- Mock
	- AWSサービス
	- VPCリンク
- リクエスト/レスポンス変換
	- マッピングテンプレートを使用してHTTPリクエストやAPIのレスポンス形式を変換する。JSON→XMLなど。
- プロキシ統合
	- マッピングテンプレートを使用せず、バックエンドが返すレスポンスをそのままAPIのレスポンスとできる
	- Lambdaプロキシ統合
		- API Gatewayと[[AWS Lambda]]を連携させる統合方式の一つで、最も一般的で推奨される方法
		- API Gatewayが受け取ったHTTPリクエストのすべての情報（ヘッダー、クエリパラメータ、パスパラメータ、ボディなど）を、JSONオブジェクトとしてLambda関数に渡す
		- Lambda関数はstatusCode, header, bodyプロパティを持つJSONでレスポンスを返す必要がある
	- HTTPプロキシ統合
		- バックエンドから返されたHTTPレスポンスデータをそのまま統合レスポンスのデータとして返す

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

### 使用量プラン
- APIに使用量（スロットリングとクォータ）プランの設定ができる。
- 設定方法
	- 使用量プランを定義し、APIステージに対して紐づける。API設定で「APIキーの必要性」をtrueにする。APIキーを定義し、使用量プランに関連付け、APIクライアント開発者に提供する。
### ログ
- API/ステージ単位で「実行ログ」と「アクセスログ」を[[CloudWatch Logs]]に出力可能

- キャッシュの設定が可能
- APIごとにリソースポリシーを設定可能
	- アクセス元の制限もできる
		- 指定AWSアカウントのユーザー
		- 指定ソースIPアドレス範囲またはCIDRブロック
		- 指定VPCまたは任意アカウントのVPCエンドポイント
- カナリアリリース
	- 各ステージ（メイン）に紐づく「Canary」ステージを作成し、リクエストを指定の比率でCanaryステージへ流すことが可能
- [[AWS WAF]]連携
	- REST APIではAPIステージにAWS WAFのWebACLを指定可能
- [[AWS X-Ray]]連携
	- REST APIではAPIステージのログ設定としてAWS X-Rayへの連携によるリクエストのトレースと分析およびデバッグが可能
	- X-Rayへの連携を有効化するとX-Rayコンソールにトレースデータの表示・可視化がされる
- 