---
tags:
  - Linux/コマンド
overview: データの末尾の表示
---
```bash
tail [option] [file]

# -n 指定した行数分末尾から表示
$ tail -n 5 /etc/services
 11469	axio-disc       35100/udp               # Axiomatic discovery protocol
 11470	pmwebapi        44323/tcp               # Performance Co-Pilot client HTTP API
 11471	cloudcheck-ping 45514/udp               # ASSIA CloudCheck WiFi Management keepalive
 11472	cloudcheck      45514/tcp               # ASSIA CloudCheck WiFi Management System
 11473	spremotetablet  46998/tcp               # Capture handwritten signatures
```