- [[名前解決]]を確認するためのコマンド
```bash
$ nslookup google.com
Server:		240d:1e:2d1:3000:260:6bff:fe50:56ee # 問い合わせたDNSサーバー
Address:	240d:1e:2d1:3000:260:6bff:fe50:56ee#53

Non-authoritative answer: # DNSサーバーのキャッシュ情報でレスポンスされた
Name:	google.com # 検索したドメイン名
Address: 142.251.222.46 # 解決されたIPアドレス

```