---
tags:
  - Linux/コマンド
overview: テキストファイルのソート
---
```bash
sort [option] [file]

# -k n   n列のデータをソートする
# -t 文字列  指定した文字列を列の区切りとして使用する。デフォルトでは空白文字
$ tail -5 /etc/passwd | sort -k 3 -t : # ":"を区切り文字とした3番目の値（ユーザーID）でソートする
linuc:x:1000:1000::/home/linuc:/bin/bash
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-oom:x:995:995:systemd Userspace OOM Killer:/:/usr/sbin/nologin


# -n     数値としてソートする
tail -5 /etc/passwd | sort -k 3 -t : -n
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-oom:x:995:995:systemd Userspace OOM Killer:/:/usr/sbin/nologin
linuc:x:1000:1000::/home/linuc:/bin/bash

# -r     逆順でソートする
```