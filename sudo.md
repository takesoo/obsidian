---
tags:
  - Linux/コマンド
overview: root権限でコマンドを実行する
---
- 普段の作業は一般ユーザーで行い、必要に応じてsudoでシステム管理用のコマンドを実行する。
- 実行権限とユーザーのパスワード入力が必要。入力後５分間は省略できる。
	- `/etc/sudoers`で以下のようにするとパスワード入力が不要になる
```diff
## Allows people in group wheel to run all commands
- %wheel  ALL=(ALL)       ALL
+ #%wheel  ALL=(ALL)       ALL

## Same thing without a password
- # %wheel        ALL=(ALL)       NOPASSWD: ALL
+ %wheel        ALL=(ALL)       NOPASSWD: ALL
```
- デフォルトとして、[[Linux#wheelグループ|wheel]]グループに所属しているユーザーに実行を許可する
	- `/etc/sudoers`などの設定ファイルで、ユーザーに対してsudo権限が直接付与されている場合もあるが、wheelグループに所属させるのが正しい。
```bash
sudo command

$ tail /etc/shadow
tail: cannot open '/etc/shadow' for reading: Permission denied

$ sudo tail /etc/shadow
[sudo] password for linuc:
operator:*:19816:0:99999:7:::
games:*:19816:0:99999:7:::
ftp:*:19816:0:99999:7:::
nobody:*:19816:0:99999:7:::
systemd-coredump:!!:20045::::::
dbus:!!:20045::::::
tss:!!:20045::::::
systemd-oom:!!:20045::::::
sshd:!!:20076::::::
linuc:$6$rounds=100000$Al8GXzyDSRKvWWWa$nzNi0rp2zwsWNguBIq/h2SjOaYJQjm/4Q90.N6hQnnqUmIsq8BbfGAxbp4mDlBfxSPCB8NCNM0a/z11qLLcYm.:20079:0:99999:7:::

$ sudo tail /etc/shadow
operator:*:19816:0:99999:7:::
games:*:19816:0:99999:7:::
ftp:*:19816:0:99999:7:::
nobody:*:19816:0:99999:7:::
systemd-coredump:!!:20045::::::
dbus:!!:20045::::::
tss:!!:20045::::::
systemd-oom:!!:20045::::::
sshd:!!:20076::::::
linuc:$6$rounds=100000$Al8GXzyDSRKvWWWa$nzNi0rp2zwsWNguBIq/h2SjOaYJQjm/4Q90.N6hQnnqUmIsq8BbfGAxbp4mDlBfxSPCB8NCNM0a/z11qLLcYm.:20079:0:99999:7:::
```