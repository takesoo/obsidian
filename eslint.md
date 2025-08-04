---
tags:
  - JavaScript
  - npm
---
[Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter](https://eslint.org/)

- JavaScriptのlinter（構文解析）
- 静的解析
- `--fix`オプションで自動修正もする。ただしフォーマットは[[prettier]]に任せるのが一般的。
- Shareable config
	- 設定をobjectとしてexportしているライブラリ
	- `eslint-config-xxxx`
	- rule, pluginなどの提供ができる

## Getting Started
### v9
### v8以前
Quick start
```shell
npm init @eslint/config
```

Manual Set Up
1. `npm install --save-dev eslint`
2. `touch .eslintrc.js`
3. `npx eslint project-dir/ file1.js`

その他
- vscode-eslintをインストールするとリアルタイムに解析してくれる
- package.jsonのscriptsに`"lint"`を追加
	```json
	{
	  "scripts": {
	    "lint": "eslint src/**/*" // npm run lint で実行できる
	  }
	}
	```


## 設定ファイル
### Flat Config
- `eslint.config.*`に記述していく方式
- v9からの設定方式
- `override`や`extends`という概念が廃止された
- ruleやconfigはJSのモジュール解決の仕組みを使う
- 書き方はjs or tsやフレームワークによって異なるので、それぞれのプラグインのドキュメントを参考にした上で判断する
```js
import js from "@eslint/js";

export default [
  /**
   * 配列の要素一つ一つがconfiguration object
   * configuration objectには対象ファイル(files)、plugin, rule, ignore, その他の設定などが書ける
   * 適応するruleが重複した場合、配列の後ろのものが優先される
   */
  js.configs.recommended,
  {
    rules: {
      "no-unused-vars": "error", // ["off" | "warn" | "error" ]
      "no-undef": "error",
      // フォーマット関連のルール

	  'indent': ['error', 2, {
	    SwitchCase: 1,
	    ignoredNodes: ['JSXElement', 'JSXElement > *', 'JSXAttribute', 'JSXIdentifier', 'JSXNamespacedName', 'JSXMemberExpression', 'JSXSpreadAttribute', 'JSXExpressionContainer', 'JSXOpeningElement', 'JSXClosingElement', 'JSXFragment', 'JSXOpeningFragment', 'JSXClosingFragment', 'JSXText', 'JSXEmptyExpression', 'JSXSpreadChild']
	
	}],
    }
  }
];
```
### .eslintrc.*
- `.eslintrc.*`ファイルに記述していく方式
- v8までの設定方式
```json
{
  /**
  * この設定ファイルがルートかどうか
  */
  "root": true,

  /**
  * 静的解析のルール
  */
  "rules": {
    "quotes": [2, "double"]
  },

  /**
  * npmで配布されているプラグインを元にルールの追加などを行える
  * `rules`にルールを追加するが、有効化はされない
  * プラグインのインストールの際に`plugins`への記述が必要かどうか、`extends`だけでいいかどうかは、導入するプラグインによって異なるので、都度調べる。（プラグインの内部で`plugins`の記載がある場合は、`.eslintrc.*`で`plugins`を記載する必要はない。）
  */
  "plugins": ["eslint-plugin-abcd"], // eslint-plugin- は省略可能
  
  /**
  * Shareable configを適用する時に記述する
  * e.g. `eslint-config-google`, `eslint-config-airbnb`, `plugin:react/recommended`
  * 後ろに行くほど優先的に設定が上書きされていく
  */
  "extends": [
	  "eslint-config-google", 
	  /**
	  * プラグインが提供している設定を使用する場合は`plugin:`プレフィックスを使って以下のように記載する
	  */
	  "plugin:eslint-plugin-abcd/setting_name", // eslint-plugin- は省略可能
	  "plugin:node/recommended", // プラグイン内部で`plugins: ["node"]`が定義されているため、.eslintrc.*でpluginsの記載が不要
  ],

  /**
  * あらかじめ定義されている環境ごとのグローバル変数を有効にする
  */
  "env": {
    "blowser": true, // windowなどのグローバル変数が有効になる
    "node": true
  },

  /**
  * ソースコードから抽象構文木を生成するのに利用するパーサーの指定
  * TypeScriptなど、VanillaJS以外を解析する場合に指定する。
  * JSXはデフォルトのパーサーでパースできる
  */
  "parser": ["@typescript-eslint/parser"],

  /**
  * パーサーに渡すオプション
  */
  "parserOptions": {
    "ecmaVersion": 6, // ECMAScriptのバージョン
    "sourceType": "module", // "script" | "module"; 
    "ecmaFeatures": {
      "jsx": true // JSX構文を有効化する
    }
  },

  /**
  * .ts, .vue などごとに異なるパーサールールを設定したい場合に記載するオプション
  */
  "overrides": [
    {
      files: ["*.vue"],
      extends: ["plugin:vue/recommend"],
    }
  ]
}
```

## VSCodeで自動整形
```json
// settings.json
{
  "editor.formatOnSave": true, // 保存時のフォーマットを有効にする
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit" // eslintの自動修正を有効にする
  },
  "editor.defaultFormatter": "dbaeumer.vscode-eslint" // eslintでフォーマットもする場合はこれも
}
```
## [[prettier]]との連携
- ESLintでコードチェックし、Prettierでフォーマットする
- ESLintのRulesにはフォーマット関連のものもあるが、Pritterと衝突するため無効にする。→[[eslint-config-prettier]]を使う
```shell
npm install eslint-config-prettier --save-dev
```
```json
// .eslintrc.json
{
  // extendsの最後に追加する
  "extends": ["prettier"]
}
```

## plugins
```dataview
TABLE overview
FROM #eslint/plugin
```
