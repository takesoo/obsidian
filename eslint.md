---
tags:
  - JavaScript
  - npm
---
[Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter](https://eslint.org/)

- JavaScriptのlinter（構文解析）
- 静的解析
- Rules
	- ESLintが何をチェックするかは、設定ファイルに追加したruleで決まる
	- `--fix`オプションで自動修正もする
- Plugins
	- rules、processor、envの実装や設定を提供するnpmモジュール
	- e.g. @angular-eslint/eslint-plugin
- Parsers
	- コードを抽象構文木に変換する
- Custom Processor
	- プロセッサは他の種類のファイルからJavaScriptコードを抽出し、ESLintにlintさせる。
- Extends
	- Shareable configを適用する
		- Shareable configの名前、またはpathを指定する
	- 

- Shareable config
	- 設定をobjectとしてexportしているライブラリ
	- `eslint-config-xxxx`
	- rule, pluginなどの提供ができる

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
```json
{
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
	  "plugin:eslint-plugin-abcd/setting_name" // eslint-plugin- は省略可能
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
  }
}
```

## [[prettier]]との連携
### 1. `prettier-eslint`と`prettier-eslint-cli`をインストール
```shell
npm install -D prettier-eslint prettier-eslint-cli
```
### 2. ???