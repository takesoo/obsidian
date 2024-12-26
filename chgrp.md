---
tags:
  - Linux/コマンド
overview: 所有グループの変更
---
- ==ch==ange ==gr==ou==p==
- 一般ユーザーで実行する場合、自分が所属しているグループを指定することはできる。
- 他のグループを指定する場合はroot権限が必要
```bash
chgrp group file

# -R ディレクトリ内のリソースを再起的に変更
```

```bash
$ id
uid=1000(linuc) gid=1000(linuc) groups=1000(linuc),10(wheel)
$ mkdir testdir

$ chgrp wheel testdir

$ ls -ld testdir/
drwxr-xr-x 2 linuc wheel 4096 Dec 26 00:44 testdir/
$ chgrp kvm testdir
chgrp: changing group of 'testdir': Operation not permitted
```