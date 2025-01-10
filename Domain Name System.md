---
aliases:
  - DNS
  - DNSサーバー
---
- [[IPアドレス]]の[[名前解決]]の仕組み
- DNSサーバーに対してリクエストを送ることでIPアドレスを返す
- 家庭用ネットワークの場合、ルーターは通常、[[Internet Service Provider|ISP]]から指定されたDNSサーバーを使用する

```mermaid
sequenceDiagram
participant LocalPC
participant FSR as FullServiceResolver
participant ANS as AuthoritativeNameServer

LocalPC ->> FSR: Do you know IP address of google.com?
alt has cache
	FSR -->> LocalPC: Yes, I know. It's 142.250.207.46.
else does not has cache
	FSR ->> ANS: Let me know the IP address of google.com?
	ANS -->> FSR: Sure, It's 142.250.207.46.
	FSR -->> LocalPC: It's 142.250.207.46.
end
```
Authoritative name server