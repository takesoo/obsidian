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

# cdkプロジェクトの初期化
# gitリポジトリも作成される
cdk init [project-name] --language typescript
```


---
```dataview
table
from #AWS/CDK
sort file.cday asc
```


---
[実践！AWS CDK #1 導入 | DevelopersIO](https://dev.classmethod.jp/articles/cdk-practice-1-introduction/)