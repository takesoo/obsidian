---
tags:
  - JavaScript
---
[Prettier · Opinionated Code Formatter](https://prettier.io/)

- JavaScriptのformatter

## Getting Started
1. install
```zsh
# install prettier
yarn add --dev prettier
# create .prettierrc.json
echo {}> .prettierrc.json
```

2. Setup Vscode
	1. vscode拡張機能をインストール
	2. setting.jsonに設定を追加
```json
{
  "editor.formatOnSave": true, // 保存時にフォーマットを起動する
  "editor.defaultFormatter": "esbenp.prettier-vscode" // defaultFormatterをprettierにする
}
```

3. 