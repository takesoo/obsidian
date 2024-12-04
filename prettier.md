---
tags:
  - npm
---
[Prettier · Opinionated Code Formatter](https://prettier.io/)

- code [[formatter]]

## Getting Started
1. install prettier
2. install VSCode extension
3. setting.jsonに設定を追加
```json
{
  "editor.formatOnSave": true, // 保存時にフォーマットを起動する
  "editor.defaultFormatter": "esbenp.prettier-vscode" // defaultFormatterをprettierにする
}
```

4. .prettierrcを作成