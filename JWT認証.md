## What
- [[JSON Web Token|JWT]]を利用した認証方式
- 2種類の方式
	- [[JSON Web Token|JWT]]自体を[[Cookie]]に格納し、サーバーでトークンを検証する
	- `Authorization: Bearer <token>`ヘッダーで明示的に送信する