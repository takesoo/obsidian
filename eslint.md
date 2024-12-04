---
tags:
  - JavaScript
  - npm
---
[Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter](https://eslint.org/)

- JavaScriptのlinter（構文解析）
- 静的解析

## Getting Started
1. `yarn add eslint --dev`
2. `yarn eslint --init`
3. install VSCode extension
4. package.jsonのscriptsに`"lint"`を追加

## 設定ファイル
### Flat Config
- `eslint.config.js`に記述していく方式
- v9からの設定方式
### .eslintrc.*
- `.eslintrc.*`ファイルに記述していく方式
- v8までの設定方式