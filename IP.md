---
tags:
  - Linux/パッケージ
  - Linux/コマンド
aliases:
  - iproute
overview: ネットワーク情報の表示
---
```bash
$ apt install iproute
...
$ ip -h
Usage: ip [ OPTIONS ] OBJECT { COMMAND | help }
       ip [ -force ] -batch filename
where  OBJECT := { address | addrlabel | amt | fou | help | ila | ioam | l2tp |
                   link | macsec | maddress | monitor | mptcp | mroute | mrule |
                   neighbor | neighbour | netconf | netns | nexthop | ntable |
                   ntbl | route | rule | sr | tap | tcpmetrics |
                   token | tunnel | tuntap | vrf | xfrm }
       OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |
                    -h[uman-readable] | -iec | -j[son] | -p[retty] |
                    -f[amily] { inet | inet6 | mpls | bridge | link } |
                    -4 | -6 | -M | -B | -0 |
                    -l[oops] { maximum-addr-flush-attempts } | -br[ief] |
                    -o[neline] | -t[imestamp] | -ts[hort] | -b[atch] [filename] |
                    -rc[vbuf] [size] | -n[etns] name | -N[umeric] | -a[ll] |
                    -c[olor]}
```
## IPアドレスを表示
```bash
$ ip adress
# インターフェース名と状態
# lo: ループバックインターフェース
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    # リンク情報
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    # IPアドレス情報
    # scope host: ホストローカル
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    # IPv6アドレス情報
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever

...

# eth0: Dockerがコンテナに割り当てた仮想ネットワークインターフェース。ホストのネットワークブリッジを経由して通信する。
79: eth0@if80: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 65535 qdisc noqueue state UP group default 
    link/ether ***************** brd ***************** link-netnsid 0
    inet **********/16 brd ************** scope global eth0
       valid_lft forever preferred_lft forever
```
## ルーティングの表示
- ルーティングを行うためのデフォルトのルーターのことを「デフォルトゲートウェイ」という
```bash
$ ip route
# デフォルトゲートウェイが172.21.0.1に設定されていて、ネットワークインターフェースeth0を使用する
default via 172.21.0.1 dev eth0 
# 172.21.0.0/16と通信するには、ネットワークンターフェースeth0を使用する
172.21.0.0/16 dev eth0 proto kernel scope link src 172.21.0.2
```