---
tags:
  - Linux
  - パッケージマネージャー
---
| コマンド      | ディストリビューション                      | パッケージファイル形式 |
| --------- | -------------------------------- | ----------- |
| rpm, yum  | Red Hat Enterprise Linux, CentOs | .rpm        |
| dpkg, apt | Debian GNU, Ubuntu               | .deb        |

インストール方法
	[[リポジトリ]]からインストール
		`yum install <package>`
		リポジトリからパッケージを探してインストールする
	特定のパッケージファイルのインストール
		`yum install <url>`
		指定したURLからパッケージをインストールする
		リポジトリにパッケージやバージョンがない時に使用する
	ソースコードからのインストール
		```
		// ダウンロード
		wget https://ftp.foo.org/bar.tar.gz
		// 解答
		tar -xzvf bar.tar.gz
		cd bar
		// ビルドの準備
		./configure
		// ビルド
		make
		// インストール
		make install
		```
	バイナリファイルのインストール
		```
		// ダウンロード
		wget -O foo https://github.com/foo
		// 実行権限の付与
		chmod +x foo
		// 適切なディレクトリに配置
		mv foo /urs/local/bin/
		```