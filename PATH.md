---
tags:
  - linux/環境変数
---
- コマンド探索パス（コマンドの実行ファイルを探索するパスの優先順位）を格納している環境変数
```bash
$ echo $PATH
/home/linuc/.local/bin:/home/linuc/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
# コマンドを実行すると`/home/linuc/.local/bin`, `/home/linuc/bin`, `/usr/local/bin`, `/usr/bin`, `/usr/local/sbin`, `/usr/sbin`の順にプログラムを探索する

# PATHを追加する（PATHを通す）
$ export PATH=/path/to/add:$PATH >> ~/.bashrc # $PATHの先頭にパスを追加して、~/.bashrcに追記する
$ source ~/.bashrc # bashに反映する
```
