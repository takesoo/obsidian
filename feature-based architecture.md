---
tags:
  - React
  - nextjs
aliases:
  - Feature-Driven Architecture
---
## what
- [[React]]および[[Next.js]]でのディレクトリ構成戦略のひとつ
- 「機能（≒ビジネスドメイン）」に基づいて分ける
```
src/
├── api/            
├── assets/            
├── components/      
├── contexts/          
├── features/           # 機能
│   ├── post/           # 投稿機能
│   │   └── comment/    # コメント機能
│   └── user/           # ユーザー機能
├── hooks/              
├── stores/             
├── styles/             
├── types/     
├── utils/              
...
```