---
tags:
  - Docker
---
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

