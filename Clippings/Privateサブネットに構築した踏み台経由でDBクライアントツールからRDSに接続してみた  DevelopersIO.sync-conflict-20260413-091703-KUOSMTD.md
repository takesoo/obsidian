---
Created: Invalid date
URL: https://dev.classmethod.jp/articles/db-client-through-ssm/
---
[![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/05/aws-systems-manager-ssm.png)](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2019/05/aws-systems-manager-ssm.png)

---

もう踏み台をPublicサブネットに置かなくてもいい・・・？

こんにちは。AWS事業本部トクヤマシュンです。

DBクライアントツールからPrivateサブネットのRDSに接続したい時があります。  
これは、EC2踏み台ホストをPublicサブネットに構築し、クライアントマシンとの間にSSHトンネルを形成することで実現できます。  
ただ、**踏み台をオープンにしたくない**という方もおられるのではないでしょうか。

そこで今回はSession Managerを使い、Privateサブネットに構築した踏み台経由でDBクライアントツールからRDSに接続してみます。

なお過去の弊社ブログに、Session ManagerとEC2でPrivate環境にあるRedshiftに接続したエントリもありますので、合わせてご参照ください。

## 構成

Session Managerを使って、クライアントマシンとPrivateサブネットのEC2踏み台ホストの間にSSHトンネルを構築します。  
このトンネルを経由することで、DBクライアントツールからPrivateサブネットのRDSに接続できます。

DBクライアントツールは、`MySQL Workbench`、`Sequel Ace`、`DBeaver`の3つを試します。  
検証環境は次のバージョンを使います。

- クライアントマシン
    - OS
        - macOS Big Sur Version 11.6.4
    - aws-cli
        - 2.4.16
    - Session Managerプラグイン
        - 1.2.295.0
    - DBクライアント
        - MySQL Workbench Version 8.0.28
        - Sequel Ace Version 3.5.0
        - DBeaver Version 21.3.5
- Amazon EC2
    - Amazon Linux2.0.20211223.0
- Amazon RDS
    - Aurora MySQL Version 3.01.0

## RDSの設定

### セキュリティグループの設定

EC2踏み台ホストからアクセスできるように、RDSのセキュリテイグループのインバウンドルールに  
EC2踏み台ホストのセキュリティグループからのTCP/3306ポートのアクセスを追加します。

## EC2の設定

### IAMロールの設定

EC2踏み台ホストをAWS Systems Managerのマネージドインスタンスとするため、  
IAMロールに`AmazonSSMManagedInstanceCore`を割り当てます。

### セキュリティグループの設定

Session Manager経由でEC2にSSHアクセスするため、EC2セキュリティグループのインバウンドルールは無くても構いません。

### ネットワークの設定

今回のEC2はNAT Gateway経由でAWS Systems Managerエンドポイントと通信可能なため、設定は不要です。  
もしEC2がインターネットにアクセスできない環境で行う場合は、[手順](https://aws.amazon.com/jp/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)に従ってVPCエンドポイントの作成が必要です。

## クライアントマシンの設定

Macを使用した際の設定を示します。  
AWS CLIの設定は完了しているものとします。

### Session Managerプラグイン インストール

AWS CLIを使ってAWS Systems Managerのマネージドインスタンスにアクセスするには、  
Session Managerプラグインをインストールする必要があります。

[[sessio]]を参考にインストールします。

### SSH設定

RDS向けの通信をSSHトンネル経由とするために、ポートフォワードの設定が必要となります。

`~/.ssh/config`に次の設定を記載します。 今回は、ローカルホストの`3306`ポートをRDSの`3306`ポートに転送するよう設定しています。

`~/.ssh/config`

```
host private_rds    ProxyCommand sh -c "aws ssm start-session --target EC2踏み台ホストのインスタンスID --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"    User EC2踏み台ホストのユーザー名    LocalForward 3306 RDSのエンドポイント:3306    IdentityFile EC2踏み台ホストのユーザー鍵ファイルのパス
```

次のコマンドを実行し、バックグラウンドでSSHトンネルを作成します。

```
ssh -f -N private_rds
```

以上でクライアントマシンの設定も完了です。

## DBクライアントツール

いよいよ、DBクライアントツールからPrivateサブネットのRDSに接続します。

次の通り設定することで、RDSに接続できました。

### MySQL Workbench

### Sequel Ace

### DBeaver

## 最後に

クライアントマシンのDBクライアントツールからPrivateサブネットに構築した踏み台経由でRDSへの接続を行いました。  
紹介できなかったDBクライアントツールや、Windowsクライアントマシンでも同様の設定は可能かと思います。  
自身の環境に適した設定を実施してみてください。

Session Manager登場以降、**EC2踏み台ホストをPublicサブネットに置かない選択も取り得る**ようになってきています。 本ブログがそうした運用の一助となれば幸いです。