---
tags:
  - nextjs
---
Next.jsは[[React]]の実行に必要な[[Webpack]]や[[Babel]]やらなんやらが全てパッケージされたフレームワーク。フロントエンド開発はもちろんのこと、[[API Routes]]を使ってフルスタック開発もできる。
1. **[[サーバーサイドレンダリング]] (SSR)**：Next.jsは、初回のページロード時にサーバー側でページをレンダリングする機能を提供します。これにより、クライアント側でのレンダリングと比べてSEOやパフォーマンスが向上します。
2. **[[スタティックサイトジェネレーション]] (SSG)**：ビルド時にページを静的に生成し、サーバーにホストすることで、さらに高速なページロードを実現します。
3. **[[API Routes]]**：Next.jsにはAPIルートがあり、サーバーサイドでAPIエンドポイントを簡単に定義できます。これにより、サーバーレス機能を提供し、バックエンドのロジックを統合できます。
4. **ファイルベースのルーティング**：Next.jsでは、ファイルシステムに基づいたルーティングを提供し、ディレクトリ構造に基づいて自動的にルートを作成します。
## ディレクトリ構成とRouting
- [[App Router]]と [[Pages Router]]の2種類があり、構成が異なる
	- 大規模かつ高度なルーティングが必要ならApp Routerが必要だが、そうでないならPages Routerの方が簡単。
```
tree -I node_modules
# pages routerの場合
.
├── README.md
├── components
│   ├── date.tsx
│   ├── layout.module.css
│   └── layout.tsx
├── global.d.ts
├── lib
│   └── posts.ts
├── next-env.d.ts
├── package-lock.json
├── package.json
├── pages
│   ├── _app.tsx
│   ├── api
│   │   └── hello.ts
│   ├── index.tsx
│   └── posts
│       └── [id].tsx
├── posts
│   ├── pre-rendering.md
│   └── ssg-ssr.md
├── public
│   ├── favicon.ico
│   └── images
│       └── profile.jpg
├── styles
│   ├── global.css
│   └── utils.module.css
└── tsconfig.json

10 directories, 20 files
```
- 画像などの静的ファイルはpublic/ 以下に配置
- その他のディレクトリ構成は自由だがcomponents/ にコンポーネント、styles/ にglobal.cssを置くのが一般的

## フルスタック開発
[[API Routes]]を使用してサーバーサイドロジックを実装する。