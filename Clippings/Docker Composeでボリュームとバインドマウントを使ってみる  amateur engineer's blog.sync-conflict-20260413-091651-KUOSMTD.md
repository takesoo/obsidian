---
Created: Invalid date
URL: https://amateur-engineer-blog.com/docer-compose-volumes/
カテゴリー: IT
---
[![](https://gcs.amateur-engineer-blog.com/2022/02/0b3a27d0-docer-compose-volumes.jpg)](https://gcs.amateur-engineer-blog.com/2022/02/0b3a27d0-docer-compose-volumes.jpg)

---

1. [試してみる](https://amateur-engineer-blog.com/docer-compose-volumes/#toc9)
2. [参考](https://amateur-engineer-blog.com/docer-compose-volumes/#toc10)

## はじめに

Docker Composeで**ボリューム**と**バインドマウント**を利用する方法について簡単にまとめたいと思います。

### ボリューム

ボリュームとは、Dockerコンテナで扱うデータを永続化する仕組みです。  
Dockerの中にボリュームというデータを保持する領域を確保して永続化します。

### バインドマウント

バインドマウントとは、ホスト側のディレクトリやファイルをコンテナ内にマウントすることです。  
バインドマウントによってもコンテナで扱うデータを永続化することができます。

Dockerのドキュメントより

## Docker Composeでボリュームとバインドマウント

Docker Composeでボリュームやバインドマウントを利用する場合は、以下のようにサービス設定が必要です。

名前付きボリュームを利用する場合は、追加でボリューム設定も必要になります。

```
version: "3"services:  web:    image: nginx:alpine    volumes:      - type: volume        source: mydata        target: /data        volume:          nocopy: true      - type: bind        source: ./static        target: /opt/app/static  db:    image: postgres:latest    environment:      - POSTGRES_PASSWORD=password    volumes:      - "dbdata:/var/lib/postgresql/data"volumes:  mydata:  dbdata:    external: true
```

### サービス設定

サービス設定のサブオプションとしての`volumes`は2種類の書き方があります。

### 1行で記述

下記の書式でボリュームまたはバインドマウントを設定できます。

```
[SOURCE:]TARGET[:MODE]
```

下記のように設定できます。

```
services:  service_name:    volumes:      # ボリューム      - /var/lib/mysql # パス指定のみ。Engine にボリュームを生成させます。      - datavolume:/var/lib/mysql # 名前つきボリューム。      # バインドマウント      - /opt/data:/var/lib/mysql # 絶対パスのマッピングを指定。      - ./cache:/tmp/cache # ホストからのパス指定。Compose ファイルからの相対パス。      - ~/configs:/etc/configs/:ro # ユーザーディレクトリからの相対パス。volumes:  dattavolume:
```

名前付きボリュームの場合はボリューム設定が必要になります。

### 複数行で記述

下記の項目を細かく設定して記述することもできます。

- `type` : マウントタイプ(`volume`, `bind`, `tmpfs`, `npipe`)
- `source` : マウント元
- `target` : マウントされるコンテナのパス
- `read_only` : 読み込み専用
- `bind` : バインドオプション
    - `propagation` : バインドの伝播モード
- `volume` : ボリュームオプション
    - `nocopy` : ボリューム生成時のコンテナからのデータコピーを無効
- `tmpfs` : tmpfsオプション
    - `size` : tmpfsマウントのサイズ

下記のように設定できます。

```
services:  service_name:    volumes:      - type: volume        source: mydata        target: /data        volume:          nocopy: true      - type: bind        source: ./static        target: /opt/app/staticvolumes:  mydata:
```

### ボリューム設定

ボリューム設定では、再利用可能な名前付きボリュームの生成ができます。サービス設定で名前付きボリュームを指定した場合は設定が必要になります。

以下のようにサービス設定で指定した名前付きボリュームをボリューム設定に記述します。

```
version: "3"services:  db:    image: postgres    volumes:      - data:/var/lib/postgresql/datavolumes:  data:    external: true
```

`external`を`true`にするとComposeの外部で作成されているボリュームを指定できます。

## 試してみる

実際にボリュームとバインドマウントを試してみます。

以下のような`docker-compose.yml`を作成します。

```
version: "3"services:  web:    image: nginx:alpine    volumes:      - type: volume        source: mydata        target: /data        volume:          nocopy: true      - type: bind        source: ./static        target: /opt/app/static  db:    image: postgres:latest    environment:      - POSTGRES_PASSWORD=password    volumes:      - "dbdata:/var/lib/postgresql/data"volumes:  mydata:  dbdata:    external: true
```

マウントする`static`ディレクトとその中にテキトーなファイルを作成します。

```
❯ mkdir static❯ touch static/hoge.txt
```

ディレクトリ構成は以下のようになります。

```
.├── docker-compose.yml└── static    └── hoge.txt
```

`docker-compose up -d`で実行するとコンテナとボリュームが作成されました。

```
❯ docker-compose up -d[+] Running 5/5 ⠿ Network data-vol_default  Created                    0.1s ⠿ Volume "data-vol_mydata"  Created                    0.0s ⠿ Volume "dbdata"                     Creat...                   0.0s ⠿ Container data-vol_web_1  Started                    1.0s ⠿ Container data-vol_db_1   Started                    1.0s
```

マウントしたディレクトリにあるファイルがコンテナ上でも確認できます。

```
❯ docker container exec -it data-vol_web_1 ls /opt/app/statichoge.txt
```

ボリュームを確認すると`external: true`にした方は指定したボリューム名で、そうでない方は[composeのプロジェクト名]+ボリューム名で作成されたことがわかります。

```
❯ docker volume lsDRIVER    VOLUME NAMElocal     data-vol_mydatalocal     dbdata
```

試しに`docker-compose down`でコンテナを停止してみます。

```
❯ docker-compose down[+] Running 3/3 ⠿ Container data-vol_db_1   Removed                    0.3s ⠿ Container data-vol_web_1  Removed                    0.3s ⠿ Network data-vol_default  Removed                    0.1s
```

するとコンテナは削除されましたが、ボリュームは残っています。

```
❯ docker volume lsDRIVER    VOLUME NAMElocal     data-vol_mydatalocal     dbdata
```

再度`docker-compose up -d`で実行しても、ボリュームが新たに作られることはなく再利用されています。

```
❯ docker-compose up -d[+] Running 3/3 ⠿ Network data-vol_default  Created                    0.1s ⠿ Container data-vol_db_1   Started                    1.0s ⠿ Container data-vol_web_1  Started                    1.0s❯ docker volume lsDRIVER    VOLUME NAMElocal     data-vol_mydatalocal     dbdata
```

## 参考