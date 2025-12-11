---
URL: https://dev.classmethod.jp/articles/creds-design-pattern-in-docker/
---
[![](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2013/11/docker.png)](https://d1tlzifd8jdoy4.cloudfront.net/wp-content/uploads/2013/11/docker.png)

---

この記事は公開されてから1年以上経過しています。情報が古い可能性がありますので、ご注意ください。

ども、大瀧です。  
データベースやクラウドストレージにアクセスするために、DockerコンテナでパスワードやAPIトークンキーなどのいわゆるクレデンシャル(資格)情報を扱うことがあります。これらの情報の扱い方についていくつかパターンを挙げ、考察してみたいと思います。

## TL;DR(要点)

- DockerイメージやDockerfileに埋め込むのは**アンチパターン**
- コンテナ実行時に環境変数で渡すのがメジャー。しかしクレデンシャル管理が不要になるわけではない
- コンテナ実行時に外部から動的取得するのがおすすめ。クラウドのメタデータサーバーの利用がお手軽

## クレデンシャル情報とは

クレデンシャルは、コンテナから外部のデータソースにアクセスするための資格情報を指します。典型的なクレデンシャルとして以下があります。

- DBユーザー名とDBパスワード : dbuser/dbpass
- WebサービスにアクセスするAPIトークンキー : AKIAXXXXYYYYZZZZ

一般にこれらの情報が他のユーザーに知られてしまうと、データソースやサービスへの不正アクセスに利用されデータの改ざん・情報漏洩につながるリスクがあり、厳重に管理する必要があります。

## Dockerイメージ/Dockerfileに埋め込む

従来の非コンテナ環境では、アプリケーションのコードや設定ファイルにクレデンシャルを直接記述することが多かったと思います。しかし、コンテナ内のファイルに直接記述するのは以下の理由でお勧めできません。

1. クレデンシャルを更新するたびにDockerイメージおよびコンテナを作り直すことになる
2. Docker Hubなどの共有サービスを介してDockerイメージのデータが漏洩するリスクがある [1](https://dev.classmethod.jp/articles/creds-design-pattern-in-docker/#note-128128-1)

Wordpressでのクレデンシャル設定例を以下に示します。

`wp-config.php`

```
  :/** MySQL database username */define('DB_USER', 'wp_user');/** MySQL database password */define('DB_PASSWORD', 'AktCp4x-');  :
```

このようなベタ書きは避け、なるべく他の方法を検討することをお勧めします。

## 環境変数で渡す

Dockerでは、環境変数をコンテナのプロセスに渡すことができます。コンテナのプロセスで環境変数を参照するように設定しておき、docker runコマンドで設定します。Docker Hubの[オフィシャルWordpressイメージ](https://github.com/docker-library/wordpress)から抜粋してみると、ENTRYPOINTで実行されるスクリプト内で環境変数からWordpressの構成ファイル(wp-config.php)にクレデンシャルを書き込んでいる様子がわかります。

`Dockerfile`

```
  :COPY docker-entrypoint.sh /entrypoint.sh# grr, ENTRYPOINT resets CMD nowENTRYPOINT ["/entrypoint.sh"]  :
```

`docker-entrypoint.sh`

```
set_config() {	key="$1"	value="$2"	php_escaped_value="$(php -r 'var_export($argv[1]);' "$value")"	sed_escaped_value="$(echo "$php_escaped_value" | sed 's/[\/&]/\\&/g')"	sed -ri "s/((['\"])$key\2\s*,\s*)(['\"]).*\3/\1$sed_escaped_value/" wp-config.php}WORDPRESS_DB_HOST="${MYSQL_PORT_3306_TCP\#tcp://}"set_config 'DB_HOST' "$WORDPRESS_DB_HOST"set_config 'DB_USER' "$WORDPRESS_DB_USER"set_config 'DB_PASSWORD' "$WORDPRESS_DB_PASSWORD"set_config 'DB_NAME' "$WORDPRESS_DB_NAME"  :
```

で、Dockerコンテナを実行するdocker runコマンドの--envオプションで環境変数を定義、指定します。

```
docker run -d \  --env WORDPRESS_DB_USER=wp_user \  --env WORDPRESS_DB_PASSWORD=AktCp4x- \  wordpress:latest
```

こうすることでDockerイメージからクレデンシャルを追い出すことはできますが、GKE(Goole Container Service)やAmazon ECS(EC2 Container Service)ではdocker runのオプションを事前に定義ファイルとして持たせるため、**定義ファイルに埋め込むクレデンシャルの管理をどうするか**という課題が残ります。

`例:ECSのTaskDefinition`

```
[  {    "image": "wordpress:latest",    "name": "wordpress",    "cpu": 10,    "memory": 500,    "essential": true,    "environment": [      {        "name": "WORDPRESS_DB_USER",        "value": "wp_user"      },      {        "name": "WORDPRESS_DB_PASSWORD",        "value": "AktCp4x-"      }    ],    "portMappings": [      {        "containerPort": 80,        "hostPort": 80      }    ]  }]
```

## 外部から取得する

コンテナ内部やコンテナ定義ファイルへの埋め込みではなく、**別の場所にクレデンシャルを保存しコンテナ実行時に動的に取得する**のがオススメです。S3などのオブジェクトストアに格納する方法も考えられますが、それはそれでS3へのアクセス権限を考えなければならず、ちょっと敷居が高いかもしれません。そこで手軽に利用できるものとして、クラウドのメタデータサーバーから取得できるインスタンスメタデータがあります。

### Amazon EC2

EC2ではメタデータとして、一般的なクレデンシャルを格納するのであれば**ユーザーデータ**、AWS APIへの権限であれば**IAMロール**が利用できます。

ユーザーデータは単一のテキストデータなので複数のクレデンシャル情報を渡したい場合、テキストを分割して読むなどコンテナ側での工夫が多少必要です。

### Google Compute Engine

GCEもEC2と同様に、一般的なクレデンシャル格納に利用できる**カスタムメタデータ**と、GCP APIを呼ぶための**サービスアカウント**があります。

### インスタンスメタデータの限界

インスタンスメタデータはインスタンスの単位で設定/取得するので、Dockerコンテナから利用するためには以下の懸念があります。

- ホストOSとDockerコンテナを区別できず、ホストOS向けのメタデータをコンテナからも取得できてしまう
- Dockerコンテナを区別できないので、全てのコンテナから共通のメタデータが取得できてしまうs

この辺りはDockerコンテナ向けのサービス(ECS/GKE)が各クラウドにあるので、今後の機能拡張に期待したいところです。

## まとめ

Dockerコンテナでのクレデンシャル情報の設計について、いくつかのパターンを紹介してみました。なるべくセキュアで手間のかからないクレデンシャル管理を検討してみてください！

## おまけ

EC2/ECS向けに、インスタンスとは異なるIAMロールを発行する構成を考えてみました。本日開催の[Docker Meetup Tokyo #4](http://connpass.com/event/10318/)のLTで発表するので、興味があれば[こちらのスライド](https://speakerdeck.com/takipone/ecsde-iamroruwoli-yong-suru)をご覧ください。後日ブログにまとめたいと思います。

## 脚注