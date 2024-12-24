---
tags:
  - Linux/コマンド
overview: 一時的に他のユーザーに切り替える
---
- ==s==uper ==u==ser
- 切り替え先初期値は[[Linux#root|root]]
- パスワードの入力が必要
	- パスワードが設定されてないユーザーには切り替えられない
- `-`なしだと、シェルなどの環境をそのまま引き継いで切り替える
- セキュリティ保護の観点から、rootのパスワードを設定せず、suコマンドを実行できないように設定することが多くなっている
```bash
su [-] [user]

$ su
Password: 
[root@e32341abc668 linuc]# id ←linucユーザーのシェル環境を引き継ぐので、カレントディレクトリのまま
uid=0(root) gid=0(root) groups=0(root)
[root@e32341abc668 linuc]# exit
exit

# - ログインシェルを起動してユーザーを切り替える
$ su -
Password: 
[root@e32341abc668 ~]# id ←ログインシェルが起動するので、/homeディレクトリから始まる
uid=0(root) gid=0(root) groups=0(root)
[root@e32341abc668 ~]# exit
logout
```