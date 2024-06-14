---
tags:
  - nodejs
---
[[Node.js]]プロジェクトのプロジェクト定義およびパッケージ依存関係を記述するファイル
`npm init`で作成できる
`npm install`するとpackage.jsonに書かれているパッケージすべてをインストールする
## devDependencies
開発の際に必要なパッケージ
`npm install --save-dev <package>`で追加できる
`npm install <project>`してもdependenciesに定義されているパッケージだけがインストールされ、devDependenceisに定義されているパッケージはインストールされない。
