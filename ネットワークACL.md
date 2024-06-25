---
tags:
  - AWS
aliases:
  - ACL
  - Access Control List
---
# ACLとは

> ACL（Access Control List）とは、システムやファイル、ネットワーク上のリソースなどへのアクセス可否の設定をリストとして列挙したものです。

> 特にネットワークの場合、宛先と送信元のIPアドレスおよびポート番号を条件とした上で、その条件に合致した通信の可否をACLとして設定します。このACLを使ってネットワークアクセスを制御することにより、特定のサーバーのIPアドレス宛の[パケット](https://www.ntt.com/bizon/glossary/j-h/packet.html)のみを許可する、あるいは特定の送信元IPアドレスからの[パケット](https://www.ntt.com/bizon/glossary/j-h/packet.html)はすべて破棄するなどといった設定が可能になります。

> このようなネットワークにおけるアクセス制御の仕組みはファイアウォールで実現されているほか、LinuxなどのOSでも利用することができます。

[

ACLとは？意味・定義 | ITトレンド用語 | NTTコミュニケーションズ

ACL（Access Control List）とは、システムやファイル、ネットワーク上のリソースなどへのアクセス可否の設定をリストとして列挙したものです。 特にネットワークの場合、宛先と送信元のIPアドレスおよびポート番号を条件とした上で、その条件に合致した通信の可否をACLとして設定します。このACLを使ってネットワークアクセスを制御することにより、特定のサーバーのIPアドレス宛の ...

![](https://www.ntt.com/favicon.ico)https://www.ntt.com/bizon/glossary/e-a/acl.html#:~:text=ACL%EF%BC%88Access%20Control%20List%EF%BC%89%E3%81%A8,ACL%E3%81%A8%E3%81%97%E3%81%A6%E8%A8%AD%E5%AE%9A%E3%81%97%E3%81%BE%E3%81%99%E3%80%82

![](https://www.ntt.com/content/dam/nttcom/hq/jp/bizon/images/common/banner18.jpg)](https://www.ntt.com/bizon/glossary/e-a/acl.html#:~:text=ACL%EF%BC%88Access%20Control%20List%EF%BC%89%E3%81%A8,ACL%E3%81%A8%E3%81%97%E3%81%A6%E8%A8%AD%E5%AE%9A%E3%81%97%E3%81%BE%E3%81%99%E3%80%82)

  

  

[

ネットワーク ACL を使用してサブネットへのトラフィックを制御する

ネットワークアクセスコントロールリスト (ACL) は、1 つ以上のサブネットのインバウンドトラフィックとアウトバウンドトラフィックを制御するファイアウォールとして動作する、VPC 用のセキュリティのオプションレイヤーです。セキュリティの追加レイヤーを VPC に追加するには、セキュリティグループと同様のルールを指定したネットワーク ACL をセットアップできます。セキュリティグループとネットワーク ACL の違いの詳細については、「 セキュリティグループとネットワーク ACL を比較する 」を参照してください。 ネットワーク ACL について知っておく必要がある基本的な情報を以下に示します。 VPC には、変更可能なデフォルトのネットワーク ACL が自動的に設定されます。デフォルトでは、すべてのインバウンドおよびアウトバウンドの IPv4 トラフィックと、IPv6 トラフィック (該当する場合) が許可されます。 カスタムネットワーク ACL を作成し、サブネットと関連付けることができます。デフォルトでは、各カスタムネットワーク ACL は、ルールを追加するまですべてのインバウンドトラフィックとアウトバウンドトラフィックを拒否します。 VPC 内の各サブネットにネットワーク ACL を関連付ける必要があります。ネットワーク ACL に明示的にサブネットを関連付けない場合、サブネットはデフォルトのネットワーク ACL に自動的に関連付けられます。 ネットワーク ACL を複数のサブネットに関連付けることができます。ただし、サブネットは一度に 1 つのネットワーク ACL にのみ関連付けることができます。サブネットとネットワーク ACL を関連付けると、以前の関連付けは削除されます。 ネットワーク

![](https://docs.aws.amazon.com/assets/images/favicon.ico)https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/vpc-network-acls.html



](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/vpc-network-acls.html)