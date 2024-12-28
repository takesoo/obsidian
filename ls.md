---
tags:
  - Linux/コマンド
overview: ファイルとディレクトリの参照
---
- ==l==i==s==t
```bash
ls [ディレクトリ名]

ls

ls -a # ドットファイルも表示
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc

mkdir -p dir1/dir2
ls -R dir1 # dir1以下のすべてを再起的に表示
dir1:
dir2

dir1/dir2:

$ ls -l
total 4
drwxr-xr-x 2 linuc wheel 4096 Dec 26 00:44 testdir
-rw-r--r-- 1 linuc linuc    0 Dec 27 00:22 testfile
# アクセス権 ハードリンクの数 所有ユーザー 所有グループ サイズ タイムスタンプ ファイル名
```

| 項目 | 8進数 | 内容                               |
| ---- | ----- | ---------------------------------- |
| r    | 4     | 読み込み                           |
| w    | 2     | 書き込み                           |
| x    | 1     | 実行、またはディレクトリ内に入れる |