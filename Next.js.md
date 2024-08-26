---
tags:
  - nextjs
---
Next.jsは[[React]]の実行に必要な[[Webpack]]や[[Babel]]やらなんやらが全てパッケージされたフレームワーク。フロントエンド開発はもちろんのこと、[[API Routes]]を使ってフルスタック開発もできる。
https://nextjs.org/docs/pages/building-your-application/routing
https://ja.next-community-docs.dev/docs/app-router/getting-started/

## [[App Router]] or [[Pages Router]]
- Pages Routerの方が古く、App Routerの方が後継
- ルーティングに関係するディレクトリ構成が異なる
- App Routerは[[サーバーコンポーネント|Server Components]]を使う。
- 大規模かつ高度なルーティングが必要ならApp Routerが必要だが、そうでないならPages Routerの方が簡単。
## Routingとディレクトリ構成
- ディレクトリ構成でルーティングが定義される
- App Routerならappディレクトリ以下、Pages Rouerならpagesディレクトリ以下の構成でルーティングが定義される
```
tree -I node_modules
# pages routerの場合
.
├── README.md
├── global.d.ts
├── next-env.d.ts
├── package-lock.json
├── package.json
├── pages
│   ├── _app.tsx
│   ├── index.tsx
├── public
│   ├── favicon.ico
└── tsconfig.json
```
- 画像などの静的ファイルはpublic/ 以下に配置
- その他のディレクトリ構成は自由だがcomponents/ にコンポーネント、styles/ にglobal.cssを置くのが一般的

## Rendering
- [[Next.js Pre-rendering|Pre-rendering]]
	- [[スタティックサイトジェネレーション|Static Site Generation]]
		- APIを叩く時は`getStaticProps`を使用する
	- [[サーバーサイドレンダリング|Server Side Rendering]]
		- APIっを叩く時は`getServerSideProps`を使用する
	- プリレンダリングされたHTMLに対してクライアントサイドJavaScriptを実行させる方法もある([[hydration]])
- [[クライアントサイドレンダリング|Client Side Rendering]]
	- APIを叩く時は[[SWR]]や[[tanstack query]]、[[React Query]]などを使用する
- [[Search Engine Optimization|SEO]]を気にしなくていいならCSRでいい
## フルスタック開発
[[API Routes]]を使用してサーバーサイドロジックを実装する。