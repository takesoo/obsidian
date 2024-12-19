---
tags:
  - Linux/コマンド
---
- [[デーモンプロセス]]の操作
```
[root@822619c2c890 /]# systemctl start httpd
[root@822619c2c890 /]# systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; preset: disabled)
     Active: active (running) since Thu 2024-12-19 16:44:43 UTC; 14s ago
       Docs: man:httpd.service(8)
   Main PID: 135 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec:   0 B/sec"
      Tasks: 177 (limit: 76436)
     Memory: 13.1M
        CPU: 91ms
     CGroup: /system.slice/httpd.service
             ├─135 /usr/sbin/httpd -DFOREGROUND
             ├─136 /usr/sbin/httpd -DFOREGROUND
             ├─137 /usr/sbin/httpd -DFOREGROUND
             ├─138 /usr/sbin/httpd -DFOREGROUND
             └─139 /usr/sbin/httpd -DFOREGROUND

Dec 19 16:44:43 822619c2c890 systemd[1]: Starting The Apache HTTP Server...
Dec 19 16:44:43 822619c2c890 httpd[135]: AH00558: httpd: Could not reliably determine the server's ful>
Dec 19 16:44:43 822619c2c890 systemd[1]: Started The Apache HTTP Server.
Dec 19 16:44:43 822619c2c890 httpd[135]: Server configured, listening on: port 80
lines 1-20/20 (END)
[root@822619c2c890 /]# systemctl stop httpd
```