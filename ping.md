---
tags:
  - Linux/コマンド
overview: IP通信の確認
---
- 指定したIPアドレスと通信が行えるか確認する
- [[ICMP]]の[[Echo]]を使用しており、相手のコンピュータがEchoに反応しない場合もpingが返ってこないことがあるが、IP通信ができてない訳ではないことに注意する
```bash
ping IPアドレスまたはホスト名

$ ping linuc.org
PING linuc.org (**************) 56(84) bytes of data.
64 bytes from 161.236.94.219.static.www3560.sakura.ne.jp (219.94.236.161): icmp_seq=1 ttl=63 time=7.98 ms
64 bytes from 161.236.94.219.static.www3560.sakura.ne.jp (219.94.236.161): icmp_seq=2 ttl=63 time=6.22 ms
64 bytes from 161.236.94.219.static.www3560.sakura.ne.jp (219.94.236.161): icmp_seq=3 ttl=63 time=10.3 ms
...
^C
--- linuc.org ping statistics ---
28 packets transmitted, 28 received, 0% packet loss, time 27136ms
rtt min/avg/max/mdev = 6.217/8.855/13.417/1.977 ms
```