---
URL: https://terashim.com/posts/docker-build-secret/
---
[![](https://terashim.com/img/ogp/docker-build-secret.png)](https://terashim.com/img/ogp/docker-build-secret.png)

---

本記事ではDockerにおける秘密情報を **(I) コンテナを起動する際に使用する秘密情報** と **(II) イメージをビルドする際に必要となる秘密情報** に分類して考え、特に後者を安全に取り扱うための方法について整理します。

## コンテナ起動時の秘密情報とイメージビルド時の秘密情報

**(I) コンテナを起動する際に使用する秘密情報**としては、例えば [`mysql`](https://hub.docker.com/_/mysql) [イメージ](https://hub.docker.com/_/mysql) の環境変数 `MYSQL_ROOT_PASSWORD` があります。コンテナの起動時にこの環境変数を与えると MySQL の root ユーザーのパスワードがその値になるというものです。

あるいは、例えば Laravel などのWebアプリケーションで、バックエンドDBへの接続情報を含んだ設定ファイルを Docker ホスト側に用意しておき、コンテナ起動時にその設定ファイルをマウントするというようなケースも考えられます。

いずれにしても、このような秘密情報は Docker イメージの中には含めず、コンテナの起動時に環境変数やファイルマウントの形で Docker ホストから渡すようにするのが慣例となっています。

また、このようなタイプの秘密情報を管理する仕組みとして [Docker Compose の secrets](https://docs.docker.com/compose/compose-file/#secrets) や [Kubernetes の Secret](https://kubernetes.io/docs/concepts/configuration/secret/) などがあります。 本記事ではこれ以上の詳細に立ち入りませんが、 **これらの仕組みは (II) のタイプの秘密情報を扱うためのものではありません**。

**(II) イメージをビルドする際に必要となる秘密情報**としては、例えば

- GitHub のプライベートリポジトリからソースコードを pull する際、認証のために使用するSSH秘密鍵
- Amazon S3 のプライベートなバケットに保存されたファイルをダウンロードするために使用するアクセスキー

などが考えられます。以下ではこのタイプの秘密情報の取り扱いについて考えます。

## 課題

Docker イメージのビルド時に **秘密情報を Dockerfile の ARG で与えるのは** [**Docker 公式ドキュメント**](https://docs.docker.com/engine/reference/builder/#arg) **で非推奨とされています**。 ビルドしたイメージを他人に共有すると、`ARG` で与えた変数は `docker history` で見られてしまうからです。 `Dockerfile` の `ENV` で書き込むのも、`COPY` で渡すのも同様にビルド後のイメージ内に秘密情報が残ってしまう問題があります。 後から `RUN` コマンドで削除しても途中のレイヤには残ってしまいます。

そのため、このタイプの**秘密情報をビルドされたイメージの中に残さないようにする工夫が必要**となります。

## 対策

ビルド時の秘密情報を安全に扱うには

[(1) BuildKit の](https://terashim.com/posts/docker-build-secret/#buildkit) [`RUN --mount=type=secret`](https://terashim.com/posts/docker-build-secret/#buildkit) [を使う](https://terashim.com/posts/docker-build-secret/#buildkit)[(2) 秘密情報が必要な処理はDockerホスト側で行い、](https://terashim.com/posts/docker-build-secret/#avoiding)[`docker build`](https://terashim.com/posts/docker-build-secret/#avoiding) [では秘密情報を扱わない](https://terashim.com/posts/docker-build-secret/#avoiding)[(3) マルチステージビルド を利用する](https://terashim.com/posts/docker-build-secret/#multistage-build)[(4) ネットワークを使って秘密情報を渡す](https://terashim.com/posts/docker-build-secret/#network)

の４つの方法が考えられます。

## (1) BuildKit の `RUN --mount=type=secret` を使う

BuildKit と `RUN --mount=type=secret` の機能については

- BuildKit ドキュメント [“buildkit/experimental.md at master · moby/buildkit”](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/experimental.md#run---mounttypesecret)
- Docker 公式ドキュメント [“Build Enhancements for Docker”](https://docs.docker.com/develop/develop-images/build_enhancements/#new-docker-build-secret-information)

に説明があるほか、例えば

- Akihiro Suda [“Docker 18.09 新機能 (イメージビルド&セキュリティ) - nttlabs - Medium”](https://medium.com/nttlabs/docker-v18-09-%E6%96%B0%E6%A9%9F%E8%83%BD-%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8%E3%83%93%E3%83%AB%E3%83%89-%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3-9534714c26e2)
- Masahito Zembutsu [“Dockerfileを改善するためのBest Practice 2019年版”](https://www.slideshare.net/zembutsu/dockerfile-bestpractices-19-and-advice#72)

で詳しく解説されています。

この機能を利用するための手順は

1. [BuildKit によるビルドを有効にする](https://matsuand.github.io/docs.docker.jp.onthefly/develop/develop-images/build_enhancements/#to-enable-buildkit-builds)（Windows や Mac で Docker Desktop バージョン 3.2.0 以降を使っていればデフォルトで有効になっています）。
2. `RUN --mount=type=secret ...` の文法でマウントの設定を記述する
3. `docker build` コマンドの `-secret` オプションでマウントする秘密情報を渡してビルドを実行する

というものです。

[Docker 公式ドキュメント](https://docs.docker.com/develop/develop-images/build_enhancements/#new-docker-build-secret-information) には以下のような例が載せられています。

`Dockerfile`:

```
# syntax = docker/dockerfile:1.0-experimentalFROM alpine# shows secret from default secret location:RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret# shows secret from custom secret location:RUN --mount=type=secret,id=mysecret,dst=/foobar cat /foobar
```

`docker build` コマンド:

```
docker build --no-cache --progress=plain --secret id=mysecret,src=mysecret.txt .
```

このようにシークレットをマウントするための専用機能を使うことで、ビルド中のコンテナに安全に秘密情報を渡すことができます。 また、このように記述することで秘密情報を渡すという目的が明確になり、可読性を高めて事故を起こりにくくすることができます。

しかし、残念ながら例えば次のようなケースではこの機能を利用できません:

- ビルド環境・CI/CDツールによっては BuildKit に対応していないことがあります。例えば Google Cloud の [Cloud Build](https://cloud.google.com/cloud-build?hl=ja) では BuildKit を利用できません。
- Docker Compose には BuildKit が統合されているものの、`RUN --mount=type=secret` には対応していません（2020年4月現在、GitHubの [PR](https://github.com/docker/compose/pull/7046) で開発が進められているところですがまだマージされていません）。したがって、ビルド時に `-secret` オプションで与えるマウントの設定をYAMLファイル `docker-compose.yml` で管理することはできません。

また、この文法はまだ実験的機能の位置づけとなっており、BuildKit の今後のリリースで変化する可能性があるとされています。  
［2021/03/28 追記］`RUN --mount` 命令は Docker Engine バージョン 20.10.0 から安定版となりました。またこれに伴い Dockerfile の１行目 `# syntax = docker/dockerfile:1.0-experimental` の記述は不要になりました（[リリースノート](https://docs.docker.com/engine/release-notes/#20100)）。

## (2) Docker build では秘密情報を扱わない

例えば GitHub のプライベートリポジトリからソースコードを取得したいのであれば、ビルド対象のコンテナ自身から接続するのではなく、Docker ホスト側で git clone を実行してダウンロードし、Dockerfile の COPY コマンドでビルド対象のコンテナ内に渡します。 このようにすると認証用の秘密鍵やアクセストークンはビルド対象のコンテナに渡さず Docker ホスト側でのみ使用するため、ビルドされるイメージに秘密情報が残る心配はありません。

この方法を取る場合、ビルドプロセスの中で秘密情報を利用するステップについては Dockerfile の外部で管理しなければなりません。これについては利用するビルド環境・CI/CDツール等に合わせて個別に対応する必要があります。

ビルド中に秘密情報を使用する目的は様々なので、Docker ホストで秘密情報を使用するステップと `docker build` を実行するステップとの連携方法も状況によって様々に変わり得ます。場合によってはビルド対象のコンテナで直接秘密情報を使用するよりもビルドプロセスが大きく複雑化してしまうケースもあるかもしれません。

## (3) マルチステージビルドを利用する

この方法は上記 (2) ビルドするイメージの中に秘密情報を持ち込まない方法の派生形とも言えます。

[マルチステージビルド](https://docs.docker.com/develop/develop-images/multistage-build/) を使うと、`docker build` によるビルドプロセス中で複数のイメージが作成されます。このとき最終成果物以外の中間イメージは（明示的に保存しなければ）ビルドプロセスの最後に破棄されます。秘密情報が必要な処理は中間イメージで行うようにすれば、最終成果物のイメージ内には秘密情報が残らないようにできます。

参考: Philipp Lies [“Building docker images from private git repositories using ssh login”](https://itnext.io/building-docker-images-from-private-git-repositories-using-ssh-login-433edf5a18f2)

この方法は (2) に比べて１つの Dockerfile で中間イメージと最終成果物をまとめて保守できる利点があります。また、マルチステージビルドはバージョン 17.05 以降の Docker で使えるため多くのビルド環境・CI/CDツールでサポートされています。

しかし、最終成果物となるコンテナから秘密情報に直接アクセスできず、ビルドプロセスが複雑化してしまう恐れがあるのは (2) と同様です。また (1) と比べると可読性が低く、最終成果物に秘密情報が残らないよう Dockerfile の内容に注意する必要があります。中間イメージを保存しない運用も必要になります。

### (4) ネットワークを使って秘密情報を渡す

これは Itamar Turner-Trauring 氏の記事 [“Docker build secrets, the sneaky way”](https://pythonspeed.com/articles/docker-build-secrets/) で現時点での最善策として紹介されている方法です。以下、一部抜粋して翻訳します ——

- * *

### 現状の最善策: ネットワーク

Docker build を実行するとき、 Dockerfile の RUN ステップではネットワークにアクセスすることができます。そしてネットワークと通信できるということは、ネットワーク越しに秘密情報を取得できるということです。私の知る限り、[Dockito Vault](https://github.com/dockito/vault) がこの方法を使っている最も古い例です。

ここではそれを簡略化したバージョンをご紹介します。

### 仕組み

この方法ではコンテナ・ネットワーキングを利用します。

- コンテナを起動するとき、固有のネットワーク名前空間がカーネルに作成されます。そして固有のネットワーク・インターフェースと対応するIPアドレスが割り当てられます。
- コンテナは既存のコンテナのネットワークに参加することもできます。
- docker build には `-network` という引数があり、特定のネットワークでビルドステップを実行することができます。

**つまり、秘密情報を返すためWebサーバを起動しておいて、ビルドプロセスの中でそのWebサーバと通信すれば秘密情報を取得できるということになります。**

### 例

busybox イメージを使って秘密情報を返すためのサーバを起動します。そしてそのコンテナの名前を `secrets-server` とします。

```
$ cat secret.txtgadzooks123$ docker run --name=secrets-server --rm --volume $PWD:/files \      busybox httpd -f -p 8000 -h /files
```

次に、この秘密情報を使う Dockerfile を作成します。デモのため秘密情報を標準出力に書き出しますが、もちろん実際の Dockerfile でこのようなことはしません。

```
FROM busyboxRUN echo "The secret is: " && \    wget -O - -q http://localhost:8000/secret.txt
```

そして、そのWebサーバにアクセスできるよう `secrets-server` のネットワーク名前空間でビルドを実行します

```
$ docker build --network=container:secrets-server .Sending build context to Docker daemon  3.072kBStep 1/2 : FROM busybox ---> 64f5d945efccStep 2/2 : RUN echo "The secret is: " && wget -O - -q http://localhost:8000/secret.txt ---> Running in 37db7718316aThe secret is:gadzooks123Removing intermediate container 37db7718316a ---> b464bdd511f9Successfully built b464bdd511f9
```

これでビルド中に安全に秘密情報へアクセスすることができます。

こうして取得した秘密情報をイメージレイヤーに残さないようにするため、そのRUNの実行期間を超えて永続化されたディスクに書き込むのは避けましょう。 これを実現するには `/dev/shm` に書き込むのが賢い方法です。 `/dev/shm` はインメモリのファイルシステムであり、最終イメージに残ることはないからです (そのRUNの期間を超えることもありません)。

（翻訳ここまで）

- * *

この記事をまとめるにあたって調査したところ、Docker Compose では (公式ドキュメントには記載されていないようですが) [build の設定で network を指定できる](https://github.com/docker/compose/pull/4997) ようです。そのため、Docker Compose でもネットワークを使う方法を利用することができます。

例えば `secrets-server` 起動用のYAMLファイル `docker-compose-secrets-server.yaml`

```
version: "3.7"services:  secrets-server:    image: busybox    container_name: secrets-server    command: ["httpd", "-f", "-p", "8000", "-h", "/files"]    volumes:      - ".:/files"
```

と、ビルド用のYAMLファイル `docker-compose.yaml`

```
version: "3.7"services:  example:    image: example:latest    build:      context: .      network: container:secrets-server
```

を用意しておき、ビルドする際には

```
docker-compose -f docker-compose-secrets-server.yaml up -d # secrets-server を起動docker-compose build # build を実行docker-compose -f docker-compose-secrets-server.yaml down # secrets-server を停止
```

のようにすれば同じことが実現可能です。

しかし、ビルドツールによってはネットワークを利用できないかもしれません。例えば Google Cloud Build では docker build のステップを実行中に他のコンテナを起動しておくようなことはできないので、この方法は使えません。

## まとめ

以上４つの方法を（筆者の独断を交えて）まとめて比較します

- (1) BuildKit の `RUN --mount=type=secret` を使う
    - ビルド対象のコンテナから秘密情報にファイルマウントで直接アクセスする。
    - 😊 安全性 - 高: イメージ内には確実に秘密情報が残らない。
    - 😊 理解性 - 高: 秘密情報を扱う記述が明確で間違いにくい。
    - 😊 簡潔性 - 高: ビルドプロセスを単一の Dockerfile でシンプルに保てる。
    - 😢 移植性 - 低: ビルド環境・ツールによっては対応していない。
- (2) 秘密情報は Docker build 内で扱わない
    - 秘密情報には Docker ホストからのみアクセスする。
    - 😊 安全性 - 高: イメージ内には確実に秘密情報が残らない。
    - 😊 理解性 - 高: ビルド環境・ツールを適切に利用することで工夫できる。
    - 😢 簡潔性 - 低: ビルドプロセス全体の記述は Dockerfile 内で完結しない。ビルド環境・ツールによるが複雑化しやすい。
    - 😢 移植性 - 低: ビルド環境・ツールに合わせたセットアップが必要。
- (3) マルチステージビルドを利用する
    - 秘密情報には中間イメージからアクセスする。
    - 🤔 安全性 - 中: 適切に用いれば問題ないが注意する必要がある。
    - 😢 理解性 - 低: 秘密情報を扱う記述がわかりにくい。
    - 🤔 簡潔性 - 中: 単一の Dockerfile で扱えるがステップを分ける必要がある。
    - 😊 移植性 - 高: ビルド環境・ツールにあまり依存しない。
- (4) ネットワークを使って秘密情報を渡す
    - ビルド対象のコンテナから秘密情報にネットワークで直接アクセスする。
    - 😊 安全性 - 高: イメージ内には確実に秘密情報が残らない。
    - 😊 理解性 - 高: 秘密情報を扱う記述が明確で間違いにくい。
    - 🤔 簡潔性 - 中: 単一の Dockerfile で記述できるが、ビルド時に秘密情報を渡すためのサーバを起動しておく必要がある。
    - 😢 移植性 - 低: ビルド環境・ツールによってはネットワークを利用できない。

BuildKit の `RUN --mount=type=secret` は優れた仕組みなので積極的に活用していきたいところですが、いまのところ対応していないビルドツールもあり、個人的には特に Docker Compose で利用できないのが残念なところです（2020年4月現在）。現状では

- 他のツールを使わずにローカル環境で直接 docker build するだけなら（１）を選択
- Cloud Build などを使った CI/CD パイプラインに組み込む場合は（２）を選択
- 簡単に Docker Compose だけで設定を完結させたい場合は（３）を選択

のように使い分けるのが最善かもしれません。

また Itamar Turner-Trauring 氏は Docker Compose の運用に関して最近の記事 [“Build secrets in Docker Compose, the secure way”](https://pythonspeed.com/articles/build-secrets-docker-compose/) で

- ローカル開発環境では docker-compose.yml ファイルに記載した秘密情報を `ARG` で渡してビルドする。したがってそのイメージは安全でないのでローカルマシンの外に持ち出さない。
- 本番用イメージをビルドするときは Docker Compose を使わず、BuildKit の `RUN --mount=type=secret` を使用する。

という方法を提示しています。このようにビルド方法を切り替えることも考えると良いかもしれません。

いずれせよ、BuildKit の `RUN --mount=type=secret` が安定版となり多くのツール・環境でサポートされるようになるまでの間は、個別の状況に合わせて工夫する必要がありそうです。

## 参考

- Akihiro Suda [“Docker 18.09 新機能 (イメージビルド&セキュリティ) - nttlabs - Medium”](https://medium.com/nttlabs/docker-v18-09-%E6%96%B0%E6%A9%9F%E8%83%BD-%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8%E3%83%93%E3%83%AB%E3%83%89-%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3-9534714c26e2)