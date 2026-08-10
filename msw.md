---
tags:
  - npm
---
## What
- Mock Service Worker
- フロントエンド用のAPIモックライブラリ
## Why
- ネットワーク境界でモックするので、fetch以降のサーバーレスポンスだけがモックされる。`vi.mock`だとAPIクライアントの差し替えになるので純粋なサーバーレスポンスのモックにならない。
## How
```ts
// 1. ハンドラ定義
const handlers = [
  http.get('/api/orders', () => HttpResponse.json([{ id: 1, ... }])),
]

// 2. セットアップ
const server = setupServer(...handlers)
beforeAll(() => server.listen({ onUnhandledRequest: 'error' }))
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```