---
tags:
  - AWS/Lambda
---
- サーバレスで関数を実行するサービス
- ソースコードのデプロイはZIP形式かDocker Image形式

---
- Docker Imageでアップロードする場合
	- docker build
	- docker push ECR_REPO_URI:latest
	- lambdaで↑のイメージを使用するように定義