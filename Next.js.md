---
tags:
  - nextjs
---
Next.jsは[[React]]の実行に必要な[[Webpack]]や[[Babel]]やらなんやらが全てパッケージされたフレームワーク。フロントエンド開発はもちろんのこと、[[API Routes]]を使ってフルスタック開発もできる。
https://nextjs.org/docs/pages/building-your-application/routing
https://ja.next-community-docs.dev/docs/app-router/getting-started/
## Getting Start
```bash
npx create-next-app nextjs-tutorial
✔ Would you like to use TypeScript? … No / Yes
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like to use `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to customize the default import alias (@/*)? … No / Yes
Creating a new Next.js app in /path/to/project/nextjs-tutorial.

Using npm.

Initializing project with template: default


Installing dependencies:
- react
- react-dom
- next

Installing devDependencies:
- typescript
- @types/node
- @types/react
- @types/react-dom


added 28 packages, and audited 29 packages in 2s

3 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Initialized a git repository.

Success! Created nextjs-tutorial at /path/to/project/nextjs-tutorial
```

## [[App Router]] or [[Pages Router]]
- ルーティングとレンダリング形式
- Pages Routerの方が古く、App Routerの方が後継
- ルーティングに関係するディレクトリ構成が異なる
- App Routerは[[サーバーコンポーネント|Server Components]]を使う。
- 大規模かつ高度なルーティングが必要ならApp Routerが必要だが、そうでないならPages Routerの方が簡単。
## Routingとディレクトリ構成
- ディレクトリ構成でルーティングが定義される
- [[dynamic routes]]
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
- [[Next.jsのディレクトリ構成を考えてみた-2024-07-26 19-34-44]]

## RenderingとData Fetching
- [[Next.js Pre-rendering|Pre-rendering]]
	- [[スタティックサイトジェネレーション|Static Site Generation]]
		- APIを叩く時は`getStaticProps`を使用する
		- ビルド時にサーバーサイドでデータフェッチする
		- ビルドでHTMLファイルを生成するので[[静的ホスティングサービス]]へのデプロイでOK
	- [[サーバーサイドレンダリング|Server Side Rendering]]
		- APIっを叩く時は`getServerSideProps`を使用する
		- リクエスト時にサーバーサイドでデータフェッチする
		- サーバーサイドの実行環境が必要。[[Vercel]]、[[Amplify]] + [[AWS Lambda|Lambda]]など。
	- プリレンダリングされたHTMLに対してクライアントサイドJavaScriptを実行させる[[Hydration]]という方法もある
- [[クライアントサイドレンダリング|Client Side Rendering]]
	- APIを叩く時は[[SWR]]や[[tanstack query]]、[[React Query]]などを使用する
	- リクエスト時にクライアントサイドでデータフェッチする
	- [[静的ホスティングサービス]]へのデプロイでOK
- [[Search Engine Optimization|SEO]]を気にしなくていいならCSRでいい
- いずれの場合も[[Vercel]]へのデプロイで動作する
## Styling
以下に対応している
- [[CSS Modules]]
- [[Global CSS]]
- [[Tailwind CSS]]
- [[Sass]]
- [[CSS-in-JS]]
## Built-in Components
- [[<Image>]]
- [[<Link>]]
- [[<Head>]]
- [[<Script>]]
## フルスタック開発（バックエンド実装）
[[API Routes]]を使用してサーバーサイドロジックを実装することもできる。（Next.jsにおけるフロントエンドとバックエンドの区別は曖昧なものだが、`jsx`や`tsx`ファイルをフロントエンド、それ以外をバックエンドと捉えておく。）
```ts
// pages/api/todos.ts
import type { NextApiRequest, NextApiResponse } from 'next';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'GET') {
    res.status(200).json({ message: 'Here are your todos' });
  } else {
    res.status(405).json({ message: 'Method Not Allowed' });
  }
}

```
## アーキテクチャ
ルーティングディレクトリ（[[App Router]]のappディレクトリ、[[Pages Router]]のpagesディレクトリ）には最低限の各ページファイルだけしか含めず、具体的なマークアップはcomponentsディレクトリは以下に実装する構成がよく採用される。
```
your-project
  |- components
  |- lib
  |- app
     |- dashboard
        |- page.tsx
     |- page.tsx
```
コンポーネント
[[hooks]]
[[データフェッチングライブラリ]]
[[APIクライアント]]
[[リポジトリパターン]]
エンティティ（モデル）