---
tags:
  - React
---
- https://github.com/alan2207/bulletproof-react
- Reactアプリケーションのアーキテクチャの一例。

## Application Oberview
サンプルアプリの概要
## Project Standards
- コーディング規約の遵守のために[[eslint]]を導入すること
- コードフォーマットのために[[prettier]]を導入すること
- [[TypeScript]]の導入を推奨する
- コミット前のコード検証ワークフローのために[[husky]]を導入すること
- 絶対パスでインポートすること
	- 🙅`import something from "../../../component"`
	- 🙆 `import something from "@/component"`
	- `"compilerOptions: { "baseUrl": ".", "paths": { "@/*": ["./src/*"] } }"`
- ファイル命名規則もeslintで強制する
## Project Structure
ソースコードはsrcディレクトリ以下に配置することで、フレームワークのコードと分離させる
```bash
src
|
+-- app               # application layer containing:
|   |                 # this folder might differ based on the meta framework used
|   +-- routes        # application routes / can also be pages
|   +-- app.tsx       # main application component
|   +-- provider.tsx  # application provider that wraps the entire application with different global providers - this might also differ based on meta framework used
|   +-- router.tsx    # application router configuration
+-- assets            # assets folder can contain all the static files such as images, fonts, etc.
|
+-- components        # shared components used across the entire application
|
+-- config            # global configurations, exported env variables etc.
|
+-- features          # feature based modules
|
+-- hooks             # shared hooks used across the entire application
|
+-- lib               # reusable libraries preconfigured for the application
|
+-- stores            # global state stores
|
+-- testing           # test utilities and mocks
|
+-- types             # shared types used across the application
|
+-- utils             # shared utility functions
```
機能関連のコードはfeaturesディレクトリ内に整理することで、共通コンポーネントとの混在を防いで管理と保守を容易にする。
```bash
src/features/awesome-feature
|
+-- api         # exported API request declarations and api hooks related to a specific feature
|
+-- assets      # assets folder can contain all the static files for a specific feature
|
+-- components  # components scoped to a specific feature
|
+-- hooks       # hooks scoped to a specific feature
|
+-- stores      # state stores for a specific feature
|
+-- types       # typescript types used within the feature
|
+-- utils       # utility functions for a specific feature
```
- apiディレクトリの処理が機能間で共有されるなら、apiディレクトリをfeaturesの外に配置するのもOK
- [[バレルファイル]]は[[vite]]だとパフォーマンスの低下につながるので推奨しない。
- [[import/no-restricted-paths]]でimportできるパスを制限することで、各機能の独立性を確保する。コードの依存関係も制限することができる。
## Components And Styling
### コンポーネントのベストプラクティス
- 使用場所にできるだけ近い場所に配置する
- レンダリング関数を入れ子にした大きなコンポーネントは避ける
```jsx
// this is very difficult to maintain as soon as the component starts growing
function Component() {
  function renderItems() {
    return <ul>...</ul>;
  }
  return <div>{renderItems()}</div>;
}

// extract it in a separate component
function Items() {
  return <ul>...</ul>;
}

function Component() {
  return (
    <div>
      <Items />
    </div>
  );
}
```
- コーディングスタイルに一貫性を保つ
- コンポーネントが入力として受け取るpropsの数を制限する
	- 多すぎる場合はコンポーネントを分割するか、子要素を用いる、またはスロット（子コンポーネントをpropsとして渡すこと）を介したコンポジションの使用を検討する
- 共有コンポーネントをコンポーネント・ライブラリに抽象化する(featuresからcomponentsに移動させる？)
### [[コンポーネントライブラリ]]
- スタイリングまでされている[[Chakra UI]]などのライブラリを使う
- デザインシステムが決まっているならば、[[ヘッドレスコンポーネントライブラリ]]を使用する
### スタイリングソリューション
- [[Tailwind CSS]]
- [[vanilla-extract]]
- [[Panda CSS]]
- [[CSS Modules]]
- [[styled-components]]
- [[emotion]]
### [[Storybook]]
コンポーネント単位での開発やカタログとして使う
## API Layer
- APIクライアントは単一インスタンスを使用する
- コンポーネントでリクエストを定義せず、apiディレクトリ以下に、リクエストごとに分けて定義する
	- リクエストデータとレスポンスデータの型を提供すること
	- APIクライアントインスタンスを使用したフェッチャー関数を提供すること
	- [[データフェッチングライブラリ]]を利用して構築されたフェッチャー関数を使用して、データフェッチとキャッシュ・ロジックを管理する[[hooks]]を提供すること
## State Management
### Component State
- コンポーネント内で定義する。グローバルに共有しない。
- [[useState]]
- 1つのアクションで複数の状態を更新するなど、複雑な状態管理は[[useReducer]]
### Application State
- アプリケーションのグローバルなステート管理
	- グローバルなモーダルの制御、通知、カラーモードの切り替えなど
- 最適なパフォーマンスとメンテナンスのしやすさを保証するために、ステートを必要とするコンポーネントのできるだけ近くに配置する。状態変数は不必要にグローバルにしない。
- [[useContext]]
- [[redux]] + [[redux toolkit]]
- [[jotai]]
### Server Cache State
- サーバーから取得したデータをクライアント側でローカルに保存すること。
- [[React Query]]
- [[swr]]
- [[apollo client]]
### Form State
- [[React Hook Form]]
- [[formik]]
- バリデーションライブラリ
	- [[zod]]
	- [[yup]]
### URL State
- ブラウザのアドレスバー内に保存され、操作されるデータのこと。
- [[useLocation]]
- [[useSearchParams]]
## Testing
### テストの種類
- ユニットテスト
	- アプリケーション全体で使われる共有コンポーネントや関数
	- 単一のコンポーネントの複雑なロジック
- 統合テスト
	- ユニット同士の連携を確認する
- E2E
	- アプリケーション全体としての振る舞いを確認する
### 推奨ツール
- [[vitest]]
- [[React Testing Library]]
- [[Playwright]]
- [[msw]]
## Error Handling
- API Errors
	- エラーを管理するためのインターセプターを実装する
	- エラーを知らせるトーストをトリガーしたり、許可されていないユーザーをログアウトしたり、トークンリフレッシュのリクエストを送信したり
- In App Errors
	- Reactのエラー境界を利用し、ドメインごとにエラー境界を配置する。こうすることでエラーが発生してもアプリケーション全体を中断することなく、ローカルでエラーを抑制および管理することができ、スムーズなUXを確保できる
- Error Tracking
	- [[Sentry]]使う
## Security
- Authentication
	- [[SPA]]では[[JWT]]を使うのが一般的
	- トークンは[[Cookie]]か[[LocalStorage]]に保存する
		- HttpOnly属性で設定されたCookiedに保存することで、クライアントサイドJavaScriptからアクセスできないようにする。js-cookieなどを使用する
	- [[クロスサイトスクリプティング|XSS]]対策として、[[サニタイズ]]する
	- [[OWASP Top 10 Client-Side Security Risks-2024-11-24 14-57-40]]
	- ユーザーデータはグローバルステートで管理する
		- [[react-query-auth]]、[[useContext|context]]、サードパーティ状態管理ライブラリ
- Authorization
	- [[Roll-Based Access Controll|RBAC]]
		- ロールベースの認可モデル
		- User, Admin
	- [[Precision-Based Access Controll|PBAC]]
		- 役割ベースの認可モデル
		- コンテンツの作成者とか
## Performance
### コード分割
- 必要な時に必要なコードだけをロードするように
- 理想的にはルートレベルで実装されるべきで、最初に必要なコードだけがロードされ、必要に応じて追加部分が遅延フェッチされる。
- 過剰なコード分割は避ける。全てのコードチャンクをフェッチするために必要なリクエスト数が増え、パフォーマンスの低下につながる。
### コンポーネントとステートの最適化
- 全ての状態を一つのステートにまとめない。
- 初期化に複雑な計算が必要なstateは、直接実行ではなく初期化関数を使用する
```ts
// instead of this which would be executed on every re-render:
const [state, setState] = React.useState(myExpensiveFn());

// prefer this which is executed only once:
const [state, setState] = React.useState(() => myExpensiveFn());
```
- [[useContext|context]]の前にlift-stateやchildrenでの状態管理を検討すること
- [[useContext|context]]は、テーマ、ユーザーデータなど、定速度のデータに適しており、中速/高速のデータの場合はセレクタをサポートしている[[use-context-selector]]、[[jotai]]などを検討する。
- パフォーマンスに影響するような頻繁な更新がある場合には、ランタイムスタイリングソリューション([[emotion]]など)から、ゼロランタイムスタイリングソリューション([[tailwind]]、[[vanilla-extract]]など)を検討する。
### チルドレン
- childrenを上手に分割すると、親のコンポーネントのライフサイクルと独立させ、不要な再レンダリングを回避できる。
```tsx
// Not optimized example
const App = () => <Counter />;

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount((count) => count + 1)}>
        count is {count}
      </button>
      <PureComponent /> // will rerender whenever "count" updates
    </div>
  );
};

const PureComponent = () => <p>Pure Component</p>;

// Optimized example
const App = () => (
  <Counter>
    <PureComponent />
  </Counter>
);

const Counter = ({ children }) => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount((count) => count + 1)}>
        count is {count}
      </button>
      {children} // won't rerender whenever "count" updates
    </div>
  );
};

const PureComponent = () => <p>Pure Component</p>;
```
### 画像
- [[viewport]]にない画像の遅延読み込み
- [[WebP]]などの最新の画像フォーマットを使用する
- [[srcset]]を使用して、クライアントサイドの画面サイズに最適な画像を読み込む
### Web Vital
- GoogleがWebサイトをインデックスする際に、[[Core Web Vitals]]を考慮するようになった
- [[Lighthouse]]と[[Pagespeed Insights]]に注目すること
### データプリフェッチ
- [[React Query]]の`queryClient.prefetchQuery`を使用すると、ユーザーがページに移動する前にデータをプリフェッチすることができる。データ読み込み時間を短縮できる。
## Deployment
- [[Vercel]]
- [[Netlify]]
- [[AWS]]
- [[CloudFlare]]

---
[Reactベストプラクティスの宝庫！「bulletproof-react」が勉強になりすぎる件](https://zenn.dev/manalink_dev/articles/bulletproof-react-is-best-architecture)
[本気で考えるReactのベストプラクティス！bulletproof-react2022](https://zenn.dev/t_keshi/articles/bulletproof-react-2022)