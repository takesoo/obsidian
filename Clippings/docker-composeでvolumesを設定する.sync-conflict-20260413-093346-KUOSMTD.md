---
Created: Invalid date
URL: https://zenn.dev/ajapa/articles/1369a3c0e8085d
---
[![](https://res.cloudinary.com/zenn/image/upload/s--6_bLcyHe--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:docker-compose%25E3%2581%25A7volumes%25E3%2582%2592%25E8%25A8%25AD%25E5%25AE%259A%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%2582%25E3%2581%2598%25E3%2582%2583%25E3%2581%25B1%25E3%2583%25BC%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzY2MTI5MWM1YmUuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png)](https://res.cloudinary.com/zenn/image/upload/s--6_bLcyHe--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:docker-compose%25E3%2581%25A7volumes%25E3%2582%2592%25E8%25A8%25AD%25E5%25AE%259A%25E3%2581%2599%25E3%2582%258B%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%2582%25E3%2581%2598%25E3%2582%2583%25E3%2581%25B1%25E3%2583%25BC%2Cx_203%2Cy_98/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzY2MTI5MWM1YmUuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/og-base.png)

---

【環境】  
MacBook Air (M1, 2020)  
OS: MacOS Big Sur version11.6  
Docker Desktop for Mac version4.5.0

[Dockerのvolumesを設定する](https://zenn.dev/ajapa/articles/fc1205d4bcbfe7)でホストとコンテナのvolumeをバインドさせることができました。  
今回はdocker-compose.ymlでvolumesを設定します。

# ディレクトリ構成

```
./volumes_test├── Dockerfile├── docker-compose.yml└── volumes_test_dir    └── test.txt
```

# test.txt

```
test
```

単純なtxtファイルです。

# Dockerfile

```
FROM alpine:latest
```

alpineサーバーのコンテナを作るためのDockerfileです。

# docker-compose.yml

## service内に記述

```
version: "3.8"services:  app:    build:      context: .      dockerfile: Dockerfile    volumes:      - ./volumes_test_dir:/volume_dir
```

services > app > build > contextはDockerfileのあるPathです。(今回は同じ階層)  
services > app > build > dockerfileは対象のDockerfile名です。  
appサービス内にvolumesオプションを記述しています。**[ホスト側の相対Path]:コンテナの絶対Path**  
という形で設定します。[Dockerのvolumesを設定する](https://zenn.dev/ajapa/articles/fc1205d4bcbfe7)で使ったDockerコマンド-vオプションではホスト側のvolumeは絶対Pathを記述しました。  
docker-composeの場合はdocker-compose.ymlからの相対Pathを記述します。

それではdocker-composeのrunコマンドでコンテナを立ち上げ、txtファイルが存在するか確認します。

```
% docker-compose run --rm app /bin/ash/ # lsbin          etc          lib          mnt          proc         run          srv          tmp          vardev          home         media        opt          root         sbin         sys          usr          volume_dir/ # cat volume_dir/test.txttest
```

volume_dirディレクトリのマウントが確認できました。  
永続化されているのでコンテナを破棄してもファイルは保持されます。  
またtest.txtファイルの編集はホストとコンテナで共有されます。

## service外に記述

volumesはservice外に記述することもできます。  
以下はtest-volumeという名前のvolumeを指定したdocker-compose.ymlです。

```
version: "3.8"services:  app:    build:      context: ./dockerfile_dir      dockerfile: Dockerfile    volumes:      - test-volume:/volume_dirvolumes:  test-volume:
```

service外(最下部のvolumes)にtest-volumeという名前のvolumeを記述し、そのtest-volumeをservice内(services > app > volumes)で使っています。**[volume名]:[コンテナの絶対Path]**  
という形で記述します。  
ここで定義したvolumeはdocker-compose実行時に自動で作成されます。  
それではdocker-composeを実行します。

```
% docker-compose run --rm app /bin/ash/ # lsbin         etc         lib         mnt         proc        run         srv         tmp         vardev         home        media       opt         root        sbin        sys         usr         volume_dir/ # ls volume_dir/ #
```

/volume_dirというディレクトリが作成されていました。  
今回はホストとマウントしているわけではないので中身は空ですが、volume_dir内で作成したデータはtest-volumeという名前のvolumeとして永続化しています。

## volumeの名前を指定する

ホスト側に戻りvolumeの名前を確認してみます。

```
/ # exit% docker volume lsDRIVER    VOLUME NAMElocal     volumes_test_test-volume
```

volumeが自動作成される場合は**[docker-compose.ymlのあるディレクトリ名]_[volume名]**  
という名前が自動的に割り当てられるようです。  
volumeの名前を指定するnameオプションを使ってみます。

```
version: "3.8"services:  app:    build:      context: ./dockerfile_dir      dockerfile: Dockerfile    volumes:      - test-volume:/volume_dirvolumes:  test-volume:    name: named-test-volume
```

volumeの名前が"named-test-volume"に変わったか確認します。

```
% docker-compose run --rm -d app /bin/ash52bc14c6bfd3f7c82f1a4a55e62ac2efe5ed10d7dd370b8bf2a1b7b4d11ca975% docker volume lsDRIVER    VOLUME NAMElocal     named-test-volume
```

docker-compose runの-dオプションを指定するとコンテナはバックグラウンドで起動します。  
volumeの名前は無事"named-test-volume"となりました。

## service外に記述(volumeを事前に作成)

volumeを事前に作成した上でvolumeをバインドさせることもできます。  
まずはexternal-test-volumeという名前のvolumeを事前に作成します。

```
% docker volume create external-test-volumeexternal-test-volume
```

docker-compose.ymlのvolumesにexternalオプションを追記します。

```
version: "3.8"services:  app:    build:      context: ./dockerfile_dir      dockerfile: Dockerfile    volumes:      - "external-test-volume:/volume_dir"volumes:  external-test-volume:    external: true
```

externalオプションによってdocker-composeの外で既に作成済みのvolumeを使うことができるようになります。  
services > app > volumesで指定するvolume名をexternal-test-volumeに修正しdocker-composeを実行すると、事前に作成したvolumeが有効であることが確認できます。

# volumesを複数行で書く

こちらの記事でvolumesを複数行に分けて書ける(long syntax)ことを知りました。

```
version: "3.8"services:  app:    build:      context: .      dockerfile: Dockerfile    volumes:      - type: bind        source: ./volumes_test_dir        target: /volume_dir
```

typeには、Docker上のvolumeを使う場合はvolumeを、ホストのディレクトリをコンテナにバインドさせる場合はbindを指定します。(他にもtmpfs, npipeアリ)  
bindのsourceにはホストのディレクトリを、targetにはコンテナのディレクトリを記述します。  
volumeのsourceにはvolume名を、targetにはコンテナのディレクトリを記述します。

単行(short syntax)だと自動でディレクトリを作成され予期せぬエラーが発生してしまうことがあるので、基本的にはlong syntaxで記述しておけば良いようです。

# 参考