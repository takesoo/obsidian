---
tags:
  - Linux/コマンド
overview: プロセスの終了
---
```bash
kill [options] PID

$ sudo tail -f /var/log/dnf.log &
[1] 39445

2024-12-29T04:28:41+0000 INFO Running transaction
2024-12-29T04:28:41+0000 DDEBUG RPM transaction start.
2024-12-29T04:28:41+0000 DDEBUG RPM transaction over.
2024-12-29T04:28:41+0000 DDEBUG timer: verify transaction: 9 ms
2024-12-29T04:28:41+0000 DDEBUG timer: transaction: 71 ms
2024-12-29T04:28:41+0000 DEBUG Installed: procps-ng-3.3.17-14.el9.aarch64
2024-12-29T04:28:41+0000 INFO Complete!
2024-12-29T04:28:41+0000 DDEBUG Cleaning up.
2024-12-29T04:28:41+0000 DDEBUG /var/cache/dnf/baseos-09b2e93483836b7c/packages/procps-ng-3.3.17-14.el9.aarch64.rpm removed
2024-12-29T04:28:41+0000 DDEBUG Plugins were unloaded.
$ ps aux | grep tail
root     39445  0.0  0.0  19436  7200 pts/1    S    09:59   0:00 sudo tail -f /var/log/dnf.log
root     39464  0.0  0.0   5284  2304 pts/1    S    09:59   0:00 /usr/bin/coreutils --coreutils-prog-shebang=tail /bin/tail -f /var/log/dnf.log
linuc    39868  0.0  0.0   3428  1736 pts/1    S+   09:59   0:00 grep --color=auto tail
$ kill 39445
[1]+  Terminated              sudo tail -f /var/log/dnf.log

# -シグナル番号 指定したシグナル番号のシグナルをプロセスに送信する
# 指定のない場合はデフォルトの15(TERM)が送信される
```


| シグナル番号 | シグナル名 |
| ------ | ----- |
| 1      | HUP   |
| 2      | INT   |
| 9      | KILL  |
| 15     | TERM  |
| 18     | TSTP  |
