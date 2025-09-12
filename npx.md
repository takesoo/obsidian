---
aliases:
  - node package executer
---
## what
- node modulesを実行するコマンド
- node_modules/.bin/を探し、なければnpmレジストリから一時できにダウンロードして実行する
	- npm install + npm exec + npm delete
- 環境の整理やディスクスペースの節約
- ローカルにインストールしたパッケージの場合は、フルパス指定しなくても、実行可能ファイルを検索して実行する
- 