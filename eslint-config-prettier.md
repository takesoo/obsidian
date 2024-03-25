---
tags:
  - VSCode
  - JavaScript
  - nodejs
  - npm
---
[prettier/eslint-config-prettier: Turns off all rules that are unnecessary or might conflict with Prettier.](https://github.com/prettier/eslint-config-prettier#installation)

- [[prettier]]と[[eslint]]を併用するとフォーマット規則が衝突することがあるので、それを解消するためのパッケージ
	- **ルールの無効化:** ESLintの中でPrettierと衝突する可能性のあるフォーマット関連のルールを自動的に無効化します。これにより、PrettierでフォーマットされたコードがESLintによって警告やエラーとして検出されることを防ぎます。
	- **衝突の解消:** Prettierが担当するコードスタイリングの面倒を見ることで、開発者がESLintの機能（主にコードの品質とバグの検出に焦点を当てる）に集中できるようにします。
	- **設定の簡略化:** 開発者が個別にESLintのルールを調整してPrettierとのコンフリクトを解消する必要がなくなり、設定プロセスを簡素化します。


## 設定方法
1. install and setup prettier and eslint
2. install and setup eslint-config-prettier
	1. install eslint-config-prettier
	2. eslintrcのに`prettier`を追加する


---
- infra-serviceとfront.canbright.jpはこの形を使っているのか？パッケージが見当たらないし、eslintrcに設定もなさそう？
- infra-serviceのpackages workspaceは何をしてるのか
