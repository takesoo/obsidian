---
tags:
  - 技術記事
  - 下書き
  - Qiita
---
- vercel postgresの自動バックアップは2024年6月時点では未実装
- Lambdaでpg_dumpする
- AmazonLinuxにpostgresql16をインストールできないっぽい。node:20-bookwormを使ってaws-lambda-ricを使用する方針で実装する
- .pgfileを作成することでパスワード入力をスキップできる
- Vercel Serverless Functionsではできないの？
	- 実行環境にpostgresqlをインストールする必要があるのでできない。VSFはnode.jsのランタイムを提供するだけ。

---
学んだこと
- docker マルチステージビルド
- CMDとENTRYPOINT
- 
- Lambdaの実行環境とaws-lambda-ric
- aws-lambda-ricはコンテナビルド時にnpm installする必要がある？
- OSへのパッケージインストールはVSFではできないがLambdaならできる。VSFではpg_dumpできない理由。
- tsc。typescriptのコンパイル。tsconfig.json。