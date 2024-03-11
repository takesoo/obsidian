---
tags:
  - AWS
  - AWS/CDK
---
- CDKの[[CodePipeline]]を利用したCI/CDのワークフローを構築してくれる、[[CDK Construct]]
- CDK Pipelineを使用したスタックをデプロイすると以下の処理フローがCodePipelineに作成される
	- Source：GitリポジトリのCDKソースコードを取得する
	- Build：CDKソースコードからCloudFormationのスタックテンプレートが正常に作成されるかテストする
	- UpdatePipeline：パイプラインの変更を検出して、更新する。自分で自分を更新する（self-mutate）。
	- Assets：CloudFormationのスタックテンプレートやその他のアーティファクトを作成して、S3にアップロード。
	- Deploy：S3から取得したアーティファクトをAWS環境上にデプロイ。

- CDK パイプラインには少なくとも 2 つの環境が含まれます。1 つの環境は、パイプラインがプロビジョニングされる環境です。もう 1 つの環境は、アプリケーションのスタック (または関連するスタックのグループであるステージ) をデプロイする場所です。これらの環境は同じにすることができますが、ベストプラクティスでは、異なる AWS アカウントまたはリージョンで互いにステージを分離することをお勧めします。

---
[CDK Pipelinesを用いたCI/CDパイプラインの作成](https://zenn.dev/hikapoppin/articles/ab39718866cbaf)