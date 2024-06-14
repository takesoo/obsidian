---
tags:
  - Docker
aliases:
  - マルチステージビルド
---
## ENTRYPOINT
Dockerコンテナのランタイムプロセスの開始点([[エントリーポイント]])として機能する

## CMD
実行コンテナのデフォルト引数の指定する

## ENTRYPOINTとCMDの違い
CMDはコマンドラインからオーバーライドできる。ENTRYPOINTは上書きできない。なのでコンテナのデフォルトコマンドをENTRYPOINTで記述し、デフォルト引数はCMDで記述すると、`docker run`コマンドで上書き引数を渡せるようになるので拡張性が高くなる。

## WORKDIR
`Dockerfile` 内で以降に続く `RUN` 、 `CMD` 、 `ENTRYPOINT` 、 `COPY` 、 `ADD` 命令の処理時に（コマンドを実行する場所として）使う 作業ディレクトリを指定する。内部的にmkdirもしてくれる。

## マルチステージビルド
`FROM`行を複数記述することで依存関係のインストールをするステージとランタイムのステージに分けてビルドすることができる。最終成果物であるイメージを小さくでき、ローカル環境も汚さずに実行できる。
```
# syntax=docker/dockerfile:1
FROM golang:1.16
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html
COPY app.go ./
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
# 一つ目のコンテナから実行に必要なものだけをコピーする
COPY --from=0 /go/src/github.com/alexellis/href-counter/app ./
CMD ["./app"]
```
