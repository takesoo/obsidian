---
tags:
  - AWS
---
## what
- AWSフルマネージド型のKey-Value型データベース
	- スケールアウト（水平スケール）を得意としている。ユーザーのテーブル操作に対してどのようなスケールが必要か自動で計算する。
- データはpartition keyによって決められた位置にsort keyの順に並んでいる
	- partition key: キーによる値へのアクセスの位置
	- sort key: モデルに対して1:nの関係
- オンデマンドキャパシティ: 自動スケーリング。アクセスパターンが予測困難なケース向け。
- プロビジョンドキャパシティ: 読み取り・書き込み容量を事前設定。予測可能なワークロード向け。
- DAX(DynamoDB Accelerator): インメモリキャッシュ
- DynamoDB Stream
	- データ変更をリアルタイムでキャプチャ
	- [[AWS Lambda|Lambda]]との連携が可能
	- [[AWS Kinesis|Kinesis]]や[[AWS Glue|Glue]]との統合
## how