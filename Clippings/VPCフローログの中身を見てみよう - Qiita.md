---
URL: https://qiita.com/miyuki_samitani/items/f83bc082156a36770828
---
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VlBDJUUzJTgzJTk1JUUzJTgzJUFEJUUzJTgzJUJDJUUzJTgzJUFEJUUzJTgyJUIwJUUzJTgxJUFFJUU0JUI4JUFEJUU4JUJBJUFCJUUzJTgyJTkyJUU4JUE2JThCJUUzJTgxJUE2JUUzJTgxJUJGJUUzJTgyJTg4JUUzJTgxJTg2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0xM2FjNzU5YjlhOTZiMTFiNDI5ZGRiZmQyN2FmNDJjNA&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbWl5dWtpX3NhbWl0YW5pJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz05MjUwMTRmNzc3MzQxMmNhY2JlMTI5YzI3NjdmNTU5Yg&blend-x=142&blend-y=491&blend-mode=normal&s=05f5725b5527b764336d7f7c0040fa14)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9VlBDJUUzJTgzJTk1JUUzJTgzJUFEJUUzJTgzJUJDJUUzJTgzJUFEJUUzJTgyJUIwJUUzJTgxJUFFJUU0JUI4JUFEJUU4JUJBJUFCJUUzJTgyJTkyJUU4JUE2JThCJUUzJTgxJUE2JUUzJTgxJUJGJUUzJTgyJTg4JUUzJTgxJTg2JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0xM2FjNzU5YjlhOTZiMTFiNDI5ZGRiZmQyN2FmNDJjNA&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbWl5dWtpX3NhbWl0YW5pJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz05MjUwMTRmNzc3MzQxMmNhY2JlMTI5YzI3NjdmNTU5Yg&blend-x=142&blend-y=491&blend-mode=normal&s=05f5725b5527b764336d7f7c0040fa14)

---

More than 1 year has passed since last update.

## そもそもVPCフローログとは？

VPC、ネットワークインターフェイスやサブネットとの間で行き来するトラフィックに関する情報をキャプチャできるようにする機能で  
CloudWatch Logs か s3で発行できます。

VPCフローログの詳細は [こちら](https://qiita.com/miyuki_samitani/items/08c4823e2a43f8a99825) をごらんください

## VPCフローログの中身を見てみよう

VPCフローログはcloudwatchのロググループで確認することが出来ます。

中身は以下のようなものが出ています。

```
タイムスタンプ                       メッセージ2021-05-17T22:48:43.000+09:00    2 XXXXXXXXX eni-aaaaaaa xx.xx.xx.xx 10.0.0.1 123 40658 17 1 76 xxxxxxxxxx xxxxxxxxxx ACCEPT OK
```

↑の中身の形式としては以下になります。  
これはVPCフローログを設定する際にAWS のデフォルト形式として設定されているものです。

```
${version} ${account-id} ${interface-id} ${srcaddr} ${dstaddr} ${srcport} ${dstport} ${protocol} ${packets} ${bytes} ${start} ${end} ${action} ${log-status}
```

- ${version} : VPCフローログのバージョン
- ${account-id} : AWSアカウントのID
- ${interface-id} : ネットワークインターフェイスのID
- ${srcaddr} : 送信元IPアドレス
- ${dstaddr} : 送信先IPアドレス
- ${srcport} : 送信元ポート
- ${dstport} : 送信先ポート
- ${protocol} : プロトコルの番号
- ${packets} : パケットの数
- ${bytes} : バイト数
- ${start} : 開始時刻(unixtime)
- ${end} : 終了時刻(unixtime)
- ${action} : アクション(ACCEPT or REJECT)
- ${log-status} : ログ(OK or NODATA or SKIPDATA)

上記を踏まえて再度メッセージを確認します

```
2                           XXXXXXXXX             eni-aaaaaaa             xx.xx.xx.xx     10.0.0.1      123         40658          17              1         76      xxxxxxxxxx   xxxxxxxxxx    ACCEPT     OKVPCフローログのバージョン   AWSアカウントのID   ネットワークインターフェイスのID     送信元IP      送信先IP   送信元ポート  送信先ポート  プロトコルの番号 パケットの数  バイト数     開始時刻      終了時刻     アクション  ログ
```

## 勉強後イメージ

わかるけど...そのまま見るのは難しい。。。

## 参考

Register as a new user and use Qiita more conveniently

1. You get articles that match your needs
2. You can efficiently read back useful information

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

Comments