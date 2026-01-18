---
tags:
  - JavaScript
---
## what
- https://hono.dev/
- web標準に則った軽量webフレームワーク
	- Ultrafast: `RegExpRouter`による高速稼働
	- Lightweight: `hono/tiny`は14kBしかなく、依存関係なし、web標準のみを使用する
	- Multi-runtime: あらゆるJavaScript ランタイムで動く
	- Batteries Included: 組み込みミドルウェア、カスタムミドルウェア、サードパーティミドルウェア、ヘルパーが含まれる
	- Delightful DX: クリーンなAPI, TypeScriptサポートによる最高の開発者体験
- ルーター、Request/Responseを扱うためのContext, ミドルウェアだけのシンプルな構成
## how
```js
import { Hono } from 'hono'
import { basicAuth } from 'hono/basic-auth'
import { upgradeWebSocket } from 'hono/cloudflare-workers'

const app = new Hono()

// return text
app.get('/', (c) => c.text('Hello Hono!'))

// return json
app.get('/api/hello', (c) => {
  return c.json({
    ok: true,
    message: 'Hello Hono!',
  })
})

// request and response
app.get('/post/:id', (c) => {
  const page = c.req.query('page') // getting a path parameter
  const id = c.req.param('id') // getting a query parameter
  c.header('X-Message', 'Hi!') // appending a Response header
  return c.text(`You want to see ${page} of ${id}`)
})

// post request
app.post('/posts', (c) => c.text('Created!', 201))
// delete request
app.delete('/posts/:id', (c) => c.text(`${c.req.param('id')} is deleted!`))

// using middleware
app.use(
  '/admin/*',
  basicAuth({
    username: 'admin',
    password: 'secret',
  })
)
app.get('/admin', (c) => {
  return c.text('You are authorized!')
})

// platform-dependent functions
app.get(
  '/ws',
  upgradeWebSocket((c) => {
    ...
  })
)
export defauilt app
```
