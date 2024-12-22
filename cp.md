---
tags:
  - Linux/コマンド
overview: ファイルのコピー
---
- ==c==o==p==y
```bash
cp [オプション] コピー元 コピー先

$ cd ~
$ mkdir work
$ cp /etc/hosts /home/linuc/work
$ ls -l /etc/hosts # サイズが同じ
-rw-r--r--. 1 root root 158  6月 23  2020 /etc/hosts
$ ls -l work
合計 4
-rw-r--r--. 1 linuc linuc 158  8月 11 12:47 hosts

$ cd work
$ cp /etc/services .
$ ls -l /etc/services
-rw-r--r--. 1 root root 692252  6月 23  2020 /etc/services
$ ls -l
合計 684
-rw-r--r--. 1 linuc linuc    158  8月 11 12:47 hosts
-rw-r--r--. 1 linuc linuc 692252  8月 11 12:48 services

# 別名でコピー
$ cp /etc/services cptest
$ ls -l
“合計 1364
-rw-r--r--. 1 linuc linuc 692252  8月 11 12:49 cptest
-rw-r--r--. 1 linuc linuc    158  8月 11 12:47 hosts
-rw-r--r--. 1 linuc linuc 692252  8月 11 12:48 services

# ファイルを上書きコピー
$ cp /etc/hosts cptest
$ ls -l
合計 688
-rw-r--r--. 1 linuc linuc    158  8月 11 12:50 cptest
-rw-r--r--. 1 linuc linuc    158  8月 11 12:47 hosts
-rw-r--r--. 1 linuc linuc 692252  8月 11 12:48 services

# ファイル名が同じだとコピーできない
cp file.txt ./
cp: 'file.txt' and './file.txt' are the same file

# -r ディレクトリとその中のファイルをコピー
$ ls origin
. .. file.txt
$ cp -r origin copy
$ ls copy
. .. file.txt
```