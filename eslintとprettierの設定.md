---
tags:
  - VSCode
  - JavaScript
---
- [[eslint]]
- [[prettier]]
- JavaScriptのプロジェクトではこの二つを組み合わせて使用することが多い

## 設定方法
### 1. prettierのインストール
1. install
```zsh
# install prettier
yarn add --dev prettier
# create .prettierrc.json
echo {}> .prettierrc.json
```
2. Setup VSCode
	1. vscode拡張機能をインストール
	2. setting.jsonに設定を追加
```json
{
  "editor.formatOnSave": true, // 保存時にフォーマットを起動する
  "editor.defaultFormatter": "esbenp.prettier-vscode" // defaultFormatterをprettierにする
}
```

### 2. eslintのインストール
1. install and configure ESLint
```zsh
yarn create @eslint/config
yarn create v1.22.11
warning package.json: No license field
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 🔨  Building fresh packages...

success Installed "@eslint/create-config@0.4.6" with binaries:
      - create-config
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · node
✔ What format do you want your config file to be in? · JSON
The config that you've selected requires the following dependencies:

@typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
✔ Would you like to install them now? · No / Yes
Successfully created .eslintrc.json file in /Users/kubotahotaka/Developments/aws/cdk/cdk_ecr
✨  Done in 109.75s.
```

2. Setup VSCode
	拡張機能をインストール
	
### 3. eslint-config-prettierの設定
1. install eslint-config-prettier
```zsh
yarn --dev add eslint-config-prettier
```
2. eslintrc.jsonにprettierを追加する
```json
"extends": [
	"eslint:recommended",
	"plugin:@typescript-eslint/recommended",
	"prettier"
],
```
