---
aliases:
  - node package manager
---
- [[Node.js]]のパッケージマネージャー

```shell

npm init
#	初期化
#	package.jsonを作成する


npm [install | i] package-name
#	パッケージインストール
#	ローカルインストール。
#	カレントディレクトリのnode_modulesにインストールされる。
#	package.jsonのdependenciesに追記される
#	
#	--save | -save | -S
#		オプションなしと同じ。npmの古いバージョンの名残。
#	
#	--save-dev | -D
#		package.jsonのdevDependenciesに追記される。dependenciesには追記されない。
#	
#	--global | -g
#		グローバルインストール

npm list
#	パッケージインストールリスト

npm [uninstall | un | unlink | remove | rm | r] package-name
#	パッケージアンインストール
```