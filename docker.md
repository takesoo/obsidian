---
tags:
  - Docker
---
## 設計哲学
- 1コンテナ=1プロセス。1つのアプリケーションプロセスをPID 1に割り当てて実行するだけのミニ環境
- プロセスを最小限にすることで、継承かつ高速な仮想化を実現する
## docker buildx
- マルチアーキテクチャに対応したdockerビルドツール
- 例えばarm64のPCでx86_64用イメージをビルドできる
- docker buildはデフォルトでローカルPCのアーキテクチャでイメージをビルドする
```dockerfile
FROM --platform linux/amd64 almalinux
```
```zsh
$ docker buildx build --platform linux/amd64 .
```

