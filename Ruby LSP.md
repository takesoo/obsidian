---
tags:
  - VSCode
  - extension
Link:
  - https://github.com/Shopify/ruby-lsp
  - https://github.com/Shopify/vscode-ruby-lsp
---
Rubyでの開発をサポートしてくれる拡張機能。
[[LSP]]

## Go To Definition
定義元への移動。クラスジャンプはできるがメソッドジャンプはできないみたい
## SemanticHighlighting セマンティック強調表示

コードが意味ごと(変数やメソッドなど)に色分けされます。
## DocumentSymbol ドキュメントシンボル

クイックオープンで「@」を入力するとファイル内に定義されてるクラス、変数、メソッドを検索できます。
## Diagnostic 診断
RuboCopの違反箇所を青い波線で教えてくれます。
## Formatting 書式設定

保存実行で RuboCopを使用してコード整形が自動で行われる機能です。  
保存時のフォーマットを有効にし、Ruby LSPをRubyフォーマッタとして登録する必要があります。(Ruby LSPインストール時に自動でsetting.jsonに設定されています。)
## タイプ時の書式
## パスの補完
## デバッグサポート
## VS Code の UI によるテストの実行とデバッグ