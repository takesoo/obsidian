## ENTRYPOINT
Dockerコンテナのランタイムプロセスの開始点([[エントリーポイント]])として機能する

## CMD
実行コンテナのデフォルト引数の指定する

## ENTRYPOINTとCMDの違い
CMDはコマンドラインからオーバーライドできる。ENTRYPOINTは上書きできない。なのでコンテナのデフォルトコマンドをENTRYPOINTで記述し、デフォルト引数はCMDで記述すると、`docker run`コマンドで上書き引数を渡せるようになるので拡張性が高くなる。

## WORKDIR
`Dockerfile` 内で以降に続く `RUN` 、 `CMD` 、 `ENTRYPOINT` 、 `COPY` 、 `ADD` 命令の処理時に（コマンドを実行する場所として）使う 作業ディレクトリを指定する。内部的にmkdirもしてくれる。