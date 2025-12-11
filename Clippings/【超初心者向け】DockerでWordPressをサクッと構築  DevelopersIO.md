---
URL: https://dev.classmethod.jp/articles/beginner-docker-wordpress/
---
[![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/docker-eyecatch.png)](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/docker-eyecatch.png)

---

この記事は公開されてから1年以上経過しています。情報が古い可能性がありますので、ご注意ください。

こんにちは！コンサル部のinomaso([@inomasosan](https://twitter.com/inomasosan))です。

絶賛コンテナ勉強中なのですが、CI/CDやIaC、ECSやEKS等で学ぶことが多くて目が回りますよね。

最近、社内の経験者に勉強方法を聞いてみたのですが、初めはそれぞれ分けて勉強した方が良いとのことでした。  
ということで、まずはDockerのチュートリアルからはじめてみることにしました。

## やること

- Docker公式ドキュメントのDocker ComposeとWordPressのクイックスタート

## この記事で学べること

- DockerでWordPress環境構築

## 環境

今回実行した環境は以下の通りです。

- macOS Catalina 10.15.7
- Docker Desktop 3.1.0

## コード

任意のディレクトリに**docker-compose.yml**を作成。 チュートリアルに従い、以下のコードを保存。

```
version: '3'services:   db:     image: mysql:5.7     volumes:       - db_data:/var/lib/mysql     restart: always     environment:       MYSQL_ROOT_PASSWORD: somewordpress       MYSQL_DATABASE: wordpress       MYSQL_USER: wordpress       MYSQL_PASSWORD: wordpress   wordpress:     depends_on:       - db     image: wordpress:latest     ports:       - "8000:80"     restart: always     environment:       WORDPRESS_DB_HOST: db:3306       WORDPRESS_DB_USER: wordpress       WORDPRESS_DB_PASSWORD: wordpressvolumes:    db_data:
```

DBのデータ保存のために**volumes**を指定していますね。  
また、**depends_on**でコンテナの依存関係を定義しているので、この例ですと**db**を起動してから、**wordpress**が起動される設定となります。

## やってみた

### 1. docker-composeを実行

**docker-compose.yml**を作成したディレクトリに移動して、実行していきます。

```
% docker-compose up -dCreating network "my_wordpress_default" with the default driverCreating volume "my_wordpress_db_data" with default driverPulling db (mysql:5.7)...5.7: Pulling from library/mysql75646c2fb410: Pull complete878f3d947b10: Pull complete1a2dd2f75b04: Pull complete8faaceef2b94: Pull completeb77c8c445ec2: Pull complete074029aeaa5f: Pull complete5a1122545c6c: Pull complete0539e5d96e0a: Pull complete5bf3befab801: Pull completea8ba39ab191f: Pull completed472d441f7e9: Pull completeDigest: sha256:dce7f54b744d09e95525ff8c767c5ba1c6a5a54d04c208fa55e3052ded713453Status: Downloaded newer image for mysql:5.7Pulling wordpress (wordpress:latest)...latest: Pulling from library/wordpressac2522cc7269: Pull complete5b2eaf06d6cf: Pull complete353d7ace7314: Pull complete37e4975b15a3: Pull completef54c541ce8ea: Pull complete4c5f70c3328b: Pull complete7721d733906f: Pull complete15a49155542c: Pull completea365bc9f6bef: Pull complete60a4e47167a5: Pull complete24b65d3ed44f: Pull complete0c1e61773eb8: Pull completec29159fb4bcc: Pull complete26fb7e4f01ad: Pull complete9c202d0c403c: Pull complete0c3ce41d4ac1: Pull complete9c5aa7a09350: Pull complete474825f9ea74: Pull complete94703b17fe04: Pull complete73c68a1f3a28: Pull complete5c2036f3fca4: Pull completeDigest: sha256:3b74157167e8fb284e108d32293be85c9e7baed7d1f5161dbdd34d75df1bdf1eStatus: Downloaded newer image for wordpress:latestCreating my_wordpress_db_1 ... doneCreating my_wordpress_wordpress_1 ... done
```

### 2. ブラウザで確認してみる

URLに**http://localhost:8000**を入力すると、WordPressの設定画面が表示されることが確認できました。

### 3. 後片付け

DBのデータ保存用のボリュームを含めて、環境を全て削除します。

```
% docker-compose down --volumesStopping my_wordpress_wordpress_1 ... doneStopping my_wordpress_db_1        ... doneRemoving my_wordpress_wordpress_1 ... doneRemoving my_wordpress_db_1        ... doneRemoving network my_wordpress_defaultRemoving volume my_wordpress_db_data
```

## まとめ

EC2等でWordPressを苦労して構築したことがある身としては、docker-compose.ymlをひとつ書くだけで実行できるのは素直に感動します。  
こういった便利な技術を課題解決のために身に付けられたらという願いを胸に、継続して勉強していこうと思います

この記事が、どなたかのお役に立てば幸いです。それでは！