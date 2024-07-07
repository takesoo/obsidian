やりたいこと
- typescript
- eslint, pretter
- swr
- prisma
- vercelデプロイ
- pathpida
- athpida
- jest
- cicd
- sentry

知りたいこと
- next.jsについて、ディレクトリ構造を理解してどこに何を追加するのか理解する
- 一通りわからないことがないくらいになりたい。
今回は勉強しないこと（別の機会にすること）
- [[React Server Component|RSC]]
- レンダリングストラテジー（静的、動的、Streaming）
実装戦略
- app router
- chakra ui
- (tailwind)
- サーバーサイドはfirebase
ブランチ戦略
- ~~tailwindリポジトリ~~
- ~~chakra ui リポジトリ~~
- next.js tutorial
	- tailwindブランチ
	- chakra uiブランチ
- fullstack next.jsは別リポジトリ？
```mermaid
gitGraph LR:

commit
branch develop
branch nextjs-tutorial
commit id: "create-next-app"
checkout develop
merge nextjs-tutorial
branch tailwind
commit id: "try tailwind"
checkout develop
branch chakra-ui
commit id: "try chakra ui"
checkout develop
merge chakra-ui
branch jest
commit id: "try jest"
checkout develop
merge jest
branch cicd
commit id: "set cicd"
checkout develop
merge cicd
```
---
## app router tutorial(途中まで)
`/app`: app routerのディレクトリ。ファイルの配置でルーティングが決まる。
`/app/lib`: 関数など
`/app/ui`: UIコンポーネント
`/app/**/page.tsx`: ルーティング対象のページファイル
`/app/**/layout.tsx`: page.tsxをラップするレイアウトファイル
`/public`: 静的アセット
`next.config.mjs(js)`:next.jsの設定ファイル

Placeholder data
	JSONフォーマットかJavaScriptオブジェクトで定義するモックデータ。データベースやAPIが用意できてない時など。

`global.css`はアプリケーション内のすべてのコンポーネントに適応される。`/app/layout.tsx`でインポートすることを推奨している。
module cssを使うことでコンポーネントごとの小さなスコープでスタイリングを定義できる
状態によってスタイリングを分岐する場合には`clsx`を使う
また、[[sass]]を使ったり[[CSS-in-JS]]ライブラリを使用するのも良い。

`/app/ui/fonts.ts`でフォントの定義をして、page.tsxなどで呼び出して使用する。フォントファイルをクライアントサイドにダウンロードさせずにサーバーサイドで適応できるためパフォーマンスの最適化につながる。

画像の表示には`next/image`コンポーネントの[[<Image>]]タグを使うと自動で最適化してくれる。

`page.tsx`はReact componentをエクスポートする特別なファイルであり、`/app`直下に配置しなければならない。`http://localhost:3000/`でこのファイルのページが表示される。

セグメント内で共通構造を共有するにはlayoutとtemplateがある。layoutの場合はlayoutインスタンスを共有するのに対してtemplateはインスタンスを共有しない。なのでlayoutはフェッチデータが引き継がれるがtemplateは引き継がない、また`useEffect()`がlayoutでは発火しないがtemplateでは発火するという違いがある

ルートレイアウトで`<head>`タグを追加するのではなく、Metadata APIを使用する方がいい

ページ遷移
	- `<Link>`コンポーネント
	- `useRouter`フック
	- ネイティブHistory API


## pages router tutorial
