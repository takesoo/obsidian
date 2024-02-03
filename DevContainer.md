---
Tags:
  - VSCode
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
## 概要
- VSCodeの開発環境をDockerコンテナ内に構築する仕組み
- 構築したコンテナによって、チームに開発環境を共有することができる

## 構成
### .devcontainer
- コンテナ開発環境(Remote Container)の定義をするディレクトリ
- devcontainer.jsonとDockerfile(またはdocker-compose.yml)が入っている
### devcontainer.json
- VSCodeがどのようにコンテナと通信するか
- どのDockerイメージを使用するか
- 拡張機能や設定
- フォワーディングするポート
- 環境変数
- コンテナ起動時に実行するコマンド
- etc...
### Dockerfileまたはdocker-compose.yml
- コンテナのイメージ定義
	- 必要なソフトウェアやツール、ライブラリのインストール
## Remote Extension Host
- ローカルのVSCodeとリモート(DevContainer)のVSCodeとの通信をするコンポーネント
	- リモートサーバー（DevContainer）との通信
	- 拡張機能の実行
	- ファイルシステムのアクセス
	- ターミナルとデバッグセッション

  

### 生成のやりかた

1. cmd + shift + p
2. Add Dev Container Configuration Files…
3. 作成したい設定を選択