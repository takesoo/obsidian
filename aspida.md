---
tags:
  - npm
---
TypeScriptでのAPIクライアントラッパー
型定義ファイルからurlやリクエストボディ、レスポンスの型を生成してくれる
src/apis/にファイルを定義するとエンドポイントを作成できる
```typescript
import aspida from "@aspida/axios";
import api from "../api/$api";
(async () => {
  const userId = 0;
  const limit = 10;
  const client = api(aspida());

  await client.v1.users.post({ body: { name: "taro" } });

  const res = await client.v1.users.get({ query: { limit } });
  console.log(res);
  // req -> GET: /v1/users/?limit=10
  // res -> { status: 200, body: [{ id: 0, name: "taro" }], headers: {...} }

  const user = await client.v1.users._userId(userId).$get();
  console.log(user);
  // req -> GET: /v1/users/0
  // res -> { id: 0, name: "taro" }
})();
```