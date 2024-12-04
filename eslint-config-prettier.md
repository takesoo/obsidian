---
tags:
  - VSCode
  - JavaScript
  - nodejs
  - npm
---
[prettier/eslint-config-prettier: Turns off all rules that are unnecessary or might conflict with Prettier.](https://github.com/prettier/eslint-config-prettier#installation)

- [[prettier]]と[[eslint]]を併用するとフォーマット規則が衝突することがあるので、それを解消するためのパッケージ
	- eslintとprettierの両方に存在しているルールを、eslint側をオフにする


## 設定方法
1. install and setup prettier and eslint
2. install and setup eslint-config-prettier
	1. install eslint-config-prettier
	2. eslintrcのに`prettier`を追加する


---
- infra-serviceとfront.canbright.jpはこの形を使っているのか？パッケージが見当たらないし、eslintrcに設定もなさそう？
- infra-serviceのpackages workspaceは何をしてるのか
