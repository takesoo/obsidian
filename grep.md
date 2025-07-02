---
tags:
  - Linux/コマンド
overview: 文字列を検索する
aliases:
  - global regular expression print
---
- 検索条件には[[正規表現]]を使用できる
```bash
grep [options] 検索条件 [file]

$ grep http /etc/services
#       http://www.iana.org/assignments/port-numbers
http            80/tcp          www www-http    # WorldWideWeb HTTP
http            80/udp          www www-http    # HyperText Transfer Protocol
http            80/sctp                         # HyperText Transfer Protocol
https           443/tcp                         # http protocol over TLS/SSL
https           443/udp                         # http protocol over TLS/SSL
https           443/sctp                        # http protocol over TLS/SSL
gss-http        488/tcp
gss-http        488/udp
# IANA claims 8008 for http-alt
webcache        8080/tcp        http-alt        # WWW caching service
webcache        8080/udp        http-alt        # WWW caching service
...

$ grep ^http /etc/services
http            80/tcp          www www-http    # WorldWideWeb HTTP
http            80/udp          www www-http    # HyperText Transfer Protocol
http            80/sctp                         # HyperText Transfer Protocol
https           443/tcp                         # http protocol over TLS/SSL
https           443/udp                         # http protocol over TLS/SSL
https           443/sctp                        # http protocol over TLS/SSL
http-mgmt       280/tcp                 # http-mgmt
http-mgmt       280/udp                 # http-mgmt
http-rpc-epmap  593/tcp                 # HTTP RPC Ep Map
http-rpc-epmap  593/udp                 # HTTP RPC Ep Map
httpx           4180/tcp                # HTTPX
httpx           4180/udp                # HTTPX
http-wmap       8990/tcp                # webmail HTTP service
http-wmap       8990/udp                # webmail HTTP service
https-wmap      8991/tcp                # webmail HTTPS service
https-wmap      8991/udp                # webmail HTTPS service

$ grep http$ /etc/services
md-cg-http      2688/tcp                # md-cf-http
md-cg-http      2688/udp                # md-cf-http
webemshttp      2851/tcp                # webemshttp
webemshttp      2851/udp                # webemshttp
plysrv-http     6770/tcp                # PolyServe http
plysrv-http     6770/udp                # PolyServe http
manyone-http    8910/tcp                # manyone-http
manyone-http    8910/udp                # manyone-http
```