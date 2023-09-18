---
Tags:
  - Docker
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
- ボリューム
    - Dockerコンテナで扱うデータを永続化する仕組み
    - コンテナ内にデータを保持する領域を確保する
- バインドマウント
    - ホスト側のディレクトリやファイルをコンテナ内にマウントすること

[[Docker Composeでボリュームとバインドマウントを使ってみる amateur engineer's blog]]

  

  

- **[ホスト側の相対Path]:コンテナの絶対Path**
- services外にvolumesを記述して**[volume名]:[コンテナの絶対Path]**

[[docker-composeでvolumesを設定する]]

  

```
services:	hoge:		volumes:			- /app/node_modules # ホストのカレントディレクトリにコンテナの/app/node_modulesをマウントする			- .:/app:cached # ホストのカレントディレクトリをコンテナの/appにマウントする			- ~/.aws:/home/.aws:cached # ホストの~/.awsをコンテナの/home/.awsにマウントする			- type: volume # 複数行で記述		    source: mydata		    target: /data		    volume:			    nocopy: true			- type: bind		    source: ./static		    target: /opt/app/staticvolumes:	mydata:# macOSやwindowsOSではvolumeにオーバーヘッドがかかるのでオプションが用意されている# consistent: 常にホストとコンテナが完全に同じ表示（default）# cached:     ホスト上の更新がコンテナ上に反映するまで、遅延が発生するのを許容# delegated:  コンテナ上の更新がホスト上に反映するまで、遅延が発生するのを許容
```