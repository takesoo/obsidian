---
Tags:
  - AWS
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
# DB一覧

- リレーショナル
- key-value
- Document
- In-memory
- Graph
- Time-series
- Ledger

---

## リレーショナル

データの正確性と一貫性に得意

スケールなどが苦手

MySQL, PostgreSQL

AWS RDS

---

## NoSQL

Not Only SQL リレーショナルSQL以外を指す

スケーラビリティが高い

RDBではカバーできない要件の際に適材適所で選択する

  

### key-value

低レイテンシー高スループット

### Document

valueがXMLまたはJSON形式

スキーマを決められない時に選ぶ

オンラインゲームの装備データなどに使われる

DynamoDB MongoDB

### In-memory

トランザクションを最大限メモリで処理する

ハイレイテンシーでリアルタイム性に優れる

キャッシュ情報など

Redies, Elasticash

### Graph

グラフ志向構造で多対多の関連の探索に優れる

SNSやレコメンドなど

  

### Time-Series

タイムスタンプをキーとする時系列データに特化したDB

IoT, 天気, 株価, ログなど

大量かつ小粒度のデータに向く

  

### Ledger

台帳DB

データの変更削除ができず、管理者権限も存在しないため、データの改竄が絶対にできない

銀行、カルテなど

  

# DBを選ぶ際

## オンプレかEC2かマネージドDBか

違いはAWSが DBサーバー運用保守をどこまでしてくれるか

マネージドDBならば開発以外の全てを肩代わり

EC2だとソフトウェアのバージョン管理やパフォーマンスもする必要がある

  

優先順位としては

1. マネージドDB
2. EC2
3. オンプレミス

要件に合わなければ1→2→3