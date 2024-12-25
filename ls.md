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
total 0
-rw-r--r-- 1 linuc linuc 0 Dec 25 00:57 test
# アクセス権 サイズ 所有ユーザー 所有グループ 作成日時 ファイル名
```