---
tags:
  - nextjs
---
## what
[[Next.js]]のプロジェクト内でフロントエンドとバックエンドのロジックを分離して管理する方法を提供するしくみ
`pages/api`ディレクトリにサーバーサイドコードを実装する
	`pages/api/users.js`は`/api/users`エンドポイントに対応する
コンポーネント（フロントエンド）からはAPIを叩く形で呼び出す

```javascript
// pages/api/users.js
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

export default async function handler(req, res) {
  if (req.method === 'GET') {
    const users = await prisma.user.findMany();
    res.status(200).json(users);
  } else if (req.method === 'POST') {
    const { name, email } = req.body;
    const newUser = await prisma.user.create({
      data: { name, email },
    });
    res.status(201).json(newUser);
  } else {
    res.status(405).end(); // Method Not Allowed
  }
}
```

## why
- サーバーサイドでトークンなどの機密情報を扱い、ブラウザから隠蔽できる
- 中間APIとして実装することでBFF
- webhookの受け口