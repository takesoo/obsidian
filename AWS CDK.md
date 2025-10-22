---
tags:
  - AWS
  - インフラ
  - IaC
  - AWS/CDK
aliases:
  - AWS Cloud Development Kit
---
## Getting Start
```shell
# aws-cdkをインストール
npm install -g aws-cdk

# CDK用のS3バケットとCfnスタックを作成する
# はじめてCDKを使うAWSアカウントに対して実行する
cdk bootstrap aws://ACCOUNT-NUMBER/REGION

mkdir sample
cd sample

# cdkプロジェクトの初期化
# gitリポジトリも作成される
cdk init [app|lib|sample-app] --language=typescript

# Cfnテンプレートをcdk.outに作成する
cdk synth

# cdkリソースをAWSにデプロイする（内部的にcdk synthも実行する）
cdk deploy

# cdkリソースを削除する
cdk destroy
```

---
## 構成要素
![[スクリーンショット 2025-09-17 10.25.34.png]]
- App
- Stack
- Construct
- Cloud Assembly
---
## ディレクトリ構成
![[Pasted image 20240330165238.png]]
- `lib/{application}-stack.ts`
	- メインスタック
- `bin/{application}.ts`
	- CDKアプリケーションのエントリポイント。`lib/{application}-stack.ts`で定義されたスタックをロードする
- `cdk.json`
	- アプリの実行方法をツールキットに指示させるためのファイル
- 
## モジュールの探し方
- [API Reference · AWS CDK](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-construct-library.html)
## リソース命名
- [AWS CDK の Construct ID はどのように命名するべきか？ | DevelopersIO](https://dev.classmethod.jp/articles/best-way-to-name-aws-cdk-construct-id/)
- CDKでリソース名を指定するのは非推奨。CDK側で自動生成する方が更新の時に何かと都合がいい。
- リソース名はConstractIdや[[CDK Pipelines]]のStage名から、[[AWS CloudFormation]]の論理idが生成され、論理idからリソース名が決定される
- ベストプラクティス
	- Construct IdはPascalCase
	- Construct IDに`Construct`や`Stack`をつけない
	- 技術詳細を含めない
		- 🙅‍♀️ `new ApiGatewaySqsLambdaConstruct('ApiGatewaySqsLambda')`
		- 論理idが変更されるとリソース再生成になるが、本番稼働中では再生成できないケースもあるため
	- 親コンストラクトで表している情報を繰り返さない
	- カスタムコンストラクトの中では`Resource`を使う。将来的にカスタムコンストラクトの外にリソース定義を移動させる想定があるなら`Default`を使う。
---
```dataview
table
from #AWS/CDK
sort file.cday asc
```


---
[実践！AWS CDK #1 導入 | DevelopersIO](https://dev.classmethod.jp/articles/cdk-practice-1-introduction/)
[AWS CDK の Construct ID はどのように命名するべきか？ | DevelopersIO](https://dev.classmethod.jp/articles/best-way-to-name-aws-cdk-construct-id/)