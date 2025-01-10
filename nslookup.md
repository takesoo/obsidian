- [[名前解決]]を確認するためのコマンド
```bash
$ nslookup google.com
Server:		240d:1e:2d1:3000:260:6bff:fe50:56ee # 問い合わせたDNSサーバー
Address:	240d:1e:2d1:3000:260:6bff:fe50:56ee#53

Non-authoritative answer: # DNSサーバーのキャッシュ情報でレスポンスされた
Name:	google.com # 検索したドメイン名
Address: 142.251.222.46 # 解決されたIPアドレス

# 名前解決に失敗する場合
$ nslookup example.com
Server:   UnKnown
Address:  192.168.1.1

*** UnKnown cant find example.com: Non-existent domain

# DNSサーバー自体が設定されていない、または到達不能な場合
$ nslookup example.com
DNS request timed out.
    timeout was 2 seconds.
Default Server: UnKnown
Address:  192.168.1.1

*** Request to UnKnown timed-out
```