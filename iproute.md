---
tags:
  - Linux/パッケージ
  - Linux/コマンド
aliases:
  - ip
overview: IPアドレスの確認
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
$ ip adress
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000 # インターフェース名と状態。ループバックインターフェース
    link/loopback ***************** brd *****************                                   # リンク情報
    inet *********/8 scope host lo                                                          # IPアドレス情報 scope host→ホストローカル
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host                                                                # IPv6アドレス情報
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip ******* brd *******
3: gre0@NONE: <NOARP> mtu 1476 qdisc noop state DOWN group default qlen 1000
    link/gre ******* brd *******
4: gretap0@NONE: <BROADCAST,MULTICAST> mtu 1462 qdisc noop state DOWN group default qlen 1000
    link/ether ***************** brd *****************
5: erspan0@NONE: <BROADCAST,MULTICAST> mtu 1450 qdisc noop state DOWN group default qlen 1000
    link/ether ***************** brd *****************
6: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip ******* brd *******
7: ip6_vti0@NONE: <NOARP> mtu 1428 qdisc noop state DOWN group default qlen 1000
    link/tunnel6 :: brd :: permaddr bacb:264b:aa3d::
8: sit0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/sit ******* brd *******
9: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN group default qlen 1000
    link/tunnel6 :: brd :: permaddr 7696:5066:bed1::
10: ip6gre0@NONE: <NOARP> mtu 1448 qdisc noop state DOWN group default qlen 1000
    link/gre6 :: brd :: permaddr eac5:4b4e:cd97::
79: eth0@if80: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 65535 qdisc noqueue state UP group default 
    link/ether ***************** brd ***************** link-netnsid 0
    inet **********/16 brd ************** scope global eth0
       valid_lft forever preferred_lft forever
$ ip r
default via ********** dev eth0 
**********/16 dev eth0 proto kernel scope link src **********
```