---
tags:
  - React
  - nextjs
---
## what
- [[React]]および[[Next.js]]でのディレクトリ構成戦略のひとつ
- [[Layered Architecture]]に基づいて分ける
- 主にバックエンドで採用され、フロントエンドで言及されることは少ない
```
src/
├── presentation/       # プレゼンテーション層
├── application/        # アプリケーション層
├── domain/             # ドメイン層
├── infrastructure/     # インフラ層
...
```
