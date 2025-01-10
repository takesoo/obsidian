---
aliases:
  - DNS
  - DNSサーバー
---
- [[IPアドレス]]の[[名前解決]]の仕組み
- DNSサーバーに対してリクエストを送ることでIPアドレスを返す
	- フルサービスリゾルバ（DNSキャッシュサーバー）
	- 権威DNSサーバー(DNSコンテンツサーバー、AuthoritativeNameServer)
		- DNSルートサーバー：フルサービスリゾルバが最初に問い合わせる先
- 家庭用ネットワークの場合、ルーターは通常、[[Internet Service Provider|ISP]]から指定されたフルサービスリゾルバを使用する

```mermaid
sequenceDiagram
participant LocalPC
participant FSR as Full Service Resolver
participant DRS as DNS Root Server
participant ANS as ".com" DNS Server

LocalPC ->> FSR: Do you know IP address of google.com?
alt has cache
	FSR -->> LocalPC: Yes, I know. It's 142.250.207.46.
else does not has cache
	FSR ->> DRS: Let me know the IP address of google.com?
	DRS -->> FSR: Ask ".com" DNS Server.
	FSR ->> ANS: Let me know the IP address of google.com?
	ANS ->> ANS: Check zonefile.
	alt resolve IP address
		ANS -->> FSR: Sure, It's 142.250.207.46.
	else
		ANS -->> FSR: Ask Child DNS server.
		FSR -->> NS: Let me know the IP address of google.com?
	end
	FSR ->> FSR: Update Cache.
	FSR -->> LocalPC: It's 142.250.207.46.
end
```
