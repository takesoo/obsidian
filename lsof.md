---
tags:
  - コマンド
---
プロセスが開いているファイルを表示する
List Open Files
## オプション
| オプション | 意味                  |
| ----- | ------------------- |
| -i    | ネットワークソケットファイルを指定する |

## 使い方
```shell
$ lsof -i:3306
COMMAND PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
mysqld  351 _mysql   20u  IPv6 0x2e1d14b48e3ca6d9      0t0  TCP *:mysql (LISTEN)
```
