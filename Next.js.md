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
- \_app.tsx: すべてのページコンポーネントの初期化に使われるコンポーネント。すべてのページで共通な処理を書く。

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
## フルスタック開発
[[API Routes]]を使用してサーバーサイドロジックを実装することもできる。
## Next.jsにおけるクライアントサイドとサーバーサイド
### クライアントサイド
[[クライアントサイドレンダリング|CSR]]のコンポーネントやクライアントサイドでのデータフェッチなど、クライアントサイドで動作する部分。
### サーバーサイド
[[API Routes]]や[[getServerSideProps]]、[[getStaticProps]]など、サーバーサイドで動作する部分。
## アーキテクチャ
### ルーティングディレクトリとコンポーネントを分ける
ルーティングディレクトリ（[[App Router]]のappディレクトリ、[[Pages Router]]のpagesディレクトリ）には最低限の各ページファイルだけしか含めず、具体的なマークアップはcomponentsディレクトリ配下に実装する構成がよく採用される。
```bash
your-project
  |- components # UIコンポーネント
  |- lib        # ユーティリティやライブラリ
  |- hooks      # カスタムフック
  |- pages      # ルーティング
     |- dashboard
        |- page.tsx
     |- index.tsx
  |- styles     # CSSやスタイル関連（CSSinModuleの場合はないこともある）
  |- types      # 型定義
```
利点
- 各ディレクトリに関心を分離
- コードの再利用性の向上
- 保守性の向上
- スケーラビリティ
- テスト容易性

さらに大規模なプロジェクトの場合、ルーティングディレクトリは単一コンポーネントを置くだけにすることもある。
```bash
your-project
  |- components # UIコンポーネント
      |- pages
	     |- top-page.tsx
  |- pages
      |- index.tsx
```
```tsx
// index.tsx
import { TopPage } from '@components/pages/top-page';

export default TopPage;
```

### リポジトリパターン
[[リポジトリパターン]]を採用して、APIクライアントを外部からDIすることもある。
## 環境変数
環境変数を `.env.local` ファイルから `process.env` にロードするためのビルトインサポートがある。
デフォルトでは、環境変数はNode.js環境でのみしようでき、ブラウザには公開されない。ブラウザに環境変数を公開するには、`NEXT_PUBLIC_`プレフィックスをつける必要がある
```bash
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```
```ts
prosess.env.NEXT_PUBLIC_ANALYTICS_ID;
```

## テスト
### UIのテスト
[[Jest]]と[[React Testing Library]], [[testing-library-jest-dom|@testing-library/jest-dom]]を使用する。
```zsh
# パッケージインストール
npm install --save-dev jest @types/jest jest-environment-jsdom @testing-library/react @testing-library/dom @testing-library/jest-dom ts-node

# jest.config.tsを作成
npm create jest@latest

# jest.setup.tsを作成
touch jest.setup.ts
```
```ts
// jest.config.ts
import "@testing-library/jest-dom";

// jest.config.ts
const config: Config = {
  ...
  // jestの実行前にjest.setup.tsを読み込むようにする
  setupFilesAfterEnv: ["<rootDir>/jest.setup.ts"],

};
```
### ユニットテスト
#### hooks
```tsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { act, renderHook, waitFor } from "@testing-library/react";
import { useTodos } from "./use-todos";
import { FirebaseTodoRepository } from "@/repositories/firebase-todo-repository";

jest.mock('@/repositories/firebase-todo-repository')

const createWrapper = () => {
  const queryClient = new QueryClient();
  return ({ children }: { children: React.ReactNode }) => {
    return (
      <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
    );
  };
};

describe("useTodos", () => {
  it("should fetch todos initially", async () => {
    const { result } = renderHook(() => useTodos(), {
      wrapper: createWrapper(),
    });
    expect(result.current.isLoading).toBe(true);
    await waitFor(() => result.current.todos && result.current.todos.length > 0)
    result.current.todos && expect(result.current.todos.length).toBeGreaterThan(0)
    });

  it('should add a new todo', async () => {
    const { result } = renderHook(() => useTodos(), {
	  wrapper: createWrapper(),
	});
	const initialTodoCount = result.current.todos?.length;
	act(() => {
	  result.current.setNewTodoTitle('New Todo')
	  result.current.addTodo()
	})
    await waitFor(() => { result.current.todos && result.current.todos.length > initialTodoCount })
	expect(result.current.todos?.some((todo) => { todo.title === 'New Todo' }))
  })
});
```

---
コンポーネント
[[hooks]]
[[データフェッチングライブラリ]]
[[APIクライアント]]
[[リポジトリパターン]]
エンティティ（モデル）