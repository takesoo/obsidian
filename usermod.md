---
tags:
  - Linux/コマンド
overview: ユーザー情報の変更（グループへの追加）
---
```bash
usermod user-name

# -g group-name プライマリグループを指定

# -G group-name サブグループを,区切りで指定
$ id sato
uid=1001(sato) gid=1001(sato) groups=1001(sato)

$ sudo usermod -G wheel sato

$ id sato
uid=1001(sato) gid=1001(sato) groups=1001(sato),10(wheel)

# -a サブグループの変更(-G)を追加として処理する
$ sudo usermod -G kvm -a sato

$ id sato
uid=1001(sato) gid=1001(sato) groups=1001(sato),10(wheel),36(kvm)
```