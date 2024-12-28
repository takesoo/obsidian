---
tags:
  - Linux/コマンド
overview: 名前解決の確認
---
```bash
dig hostname

$ dig www.linuc.org

; <<>> DiG 9.16.23-RH <<>> www.linuc.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 47012
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.linuc.org.			IN	A

;; ANSWER SECTION:
www.linuc.org.		4502	IN	A	**************(ここにIPアドレスが表示される)

;; Query time: 28 msec
;; SERVER: **********#53(**********)
;; WHEN: Sat Dec 28 08:54:30 UTC 2024
;; MSG SIZE  rcvd: 47

```