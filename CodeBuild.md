---
tags:
  - AWS
---
## what
- フルマネージドなCIサービス
- 利用した分のみの課金
- dockerコンテナ内で実行する
	- コンテナイメージはAWSから提供されたものもあれば、ユーザー独自のイメージを使用することも可能
- ビルドログはCloudWatchLogsに出力できる
## how
### buildspec.yml
- ソースディレクトリのルートに配置する
- version: buildspecのバージョン。必須
- run-as: コマンドを実行するLinuxユーザー
- env: 環境変数
	- [[AWS Secrets Manager]], [[Parameter Store]]の値を使用することもできる
- proxy: プロキシサーバー設定
- batch: バッチビルド設定。プロジェクトの同時実行と協調実行を定義できる。
	- build-graph
	- build-list
	- build-matrix
- phases: ビルドの各段階で実行するコマンド。必須
	- install
	- pre_build
	- build
	- post_build
- reports: テストレポートの作成
- artifacts: CodeBuildの出力定義
- cache: キャッシュの設定
### 機能
- ローカルマシンでビルド可能
- [[AWS Session Manager]]でビルド環境へアクセス可能
	- `codebuild-breakpoint`をbuildspec.ymlに追加する
- ステータスをGitHubのバッジで表示できる
- ステータスをAWS SNSで通知可能
- VPC内リソースへのアクセスにはNAT Gatewayをを使用する