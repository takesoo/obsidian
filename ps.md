---
tags:
  - Linux/コマンド
---
- プロセスの確認
- 表示される項目
	- PID
	- TTY
	- STAT
	- TIME
	- COMMAND

- オプション
	- a:  全てのプロセス
	- u: ユーザー名を表示
	- x: [[デーモンプロセス|デーモン]]も表示

```bash
ps [options]

$ ps
  PID TTY          TIME CMD
   12 pts/1    00:00:00 bash
22869 pts/1    00:00:00 ps

# a 制御端末のあるすべてのプロセスを表示する
$ ps a
  PID TTY      STAT   TIME COMMAND
    1 pts/0    Ss+    0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
   12 pts/1    Ss     0:00 bash --rcfile /dev/fd/63
24384 pts/1    R+     0:00 ps a

# f プロセスの親子関係を表示する
$ ps f
  PID TTY      STAT   TIME COMMAND
   12 pts/1    Ss     0:00 bash --rcfile /dev/fd/63
23167 pts/1    R+     0:00  \_ ps f # psコマンドはbashから起動された

# u 実行ユーザーを表示する
$ ps u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
linuc       12  0.0  0.0   6024  4828 pts/1    Ss   Dec28   0:00 bash --rcfile /dev/fd/63
linuc    23537  0.0  0.0   7288  2864 pts/1    R+   04:31   0:00 ps u

# x 制御端末のないバックグラウンドプロセス（デーモン）も表示する
$ ps x
  PID TTY      STAT   TIME COMMAND
   11 ?        S      0:01 sshd: linuc@pts/1 # TTYが?のものは制御端末がないのでバックグラウンドプロセス
   12 pts/1    Ss     0:00 bash --rcfile /dev/fd/63
23918 pts/1    R+     0:00 ps x
```
```bash
pstree

$ pstree
sshd───sshd───sshd───bash───pstree
```
