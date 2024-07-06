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
app/以下がapp routerのディレクトリ
