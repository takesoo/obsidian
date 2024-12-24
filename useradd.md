---
tags:
  - Linux/コマンド
overview: ユーザー作成
---
- root権限が必要
- ユーザーは必ずひとつのグループに所属し（プライマリグループ）、さらに複数のサブグループに所属することができる
- 作成したユーザーの情報は`/etc/passwd`に記述される
- `/home`直下にユーザー名でホームディレクトリが作成され、`.bashrc`などのファイルが作成される
- 作成したユーザーにパスワードを登録する場合は[[passwd]]で登録する
```bash
useradd user-name

$ sudo useradd sato
[sudo] password for linuc: 
$ grep sato /etc/passwd
sato:x:1001:1001::/home/sato:/bin/bash
$ ls -ld /home/sato
drwx------ 2 sato sato 4096 Dec 24 21:18 /home/sato

# -g group-name プライマリグループを指定

# -G group-name サブグループを,区切りで指定
```