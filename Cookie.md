## what
- webサイトがユーザーのブラウザに一時的に保存するテキストデータ
- ブラウザが[[HTTPリクエスト]]を送る時に、条件に一致するCookieを自動的に付与する。
### 属性

| 属性       | 役割                             | 値                                                                                                            |
| -------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| HttpOnly | JavaScriptからCookieを読み取れなくする    |                                                                                                              |
| Secure   | HTTPS通信でのみ送信する（localhost以外）    |                                                                                                              |
| SameSite | [[クロスサイトリクエスト]]でのCookie送信を制御する | Strict: クロスサイトリクエストでは送信しない, Lax: 原則クロスサイトリクエストでは送信しないが、通常のリンク遷移など一部で送信する, None: クロスサイトリクエストでも送信できる。Secure必須。 |
| Domain   | Cookieを送信する対象ドメインを指定する         |                                                                                                              |
| Path     | Cookieを送信するURLパスを指定する          |                                                                                                              |
| Max-Age  | Cookieの有効期間（sec）               |                                                                                                              |
| Expires  | Cookieの有効期限（日時）                |                                                                                                              |

## why
- [[HTTP]]自体には存在しない「状態」を継続して扱うため
- セッション管理
	- ログイン状態の維持
	- 言語設定の保存
	- ショッピングカートの管理
- トラッキング
## how
① ブラウザがログインAPIを呼び出す

```http
POST /api/login
Content-Type: application/json

{"email":"user@example.com","password":"..."}
```

② サーバーがCookieを発行する

```http
HTTP/1.1 200 OK
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax; Path=/
```

ブラウザは`Set-Cookie`ヘッダーを受け取り、Cookieを保存する。

③ 次回以降、ブラウザがCookieを自動送信する
```http
GET /api/profile
Cookie: session=abc123
```