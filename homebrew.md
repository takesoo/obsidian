---
Link: "[[homebrewとは何者か。仕組みについて調べてみた - Qiita]]"
---
macの[[パッケージ管理システム]]としてデファクトスタンダード。

特徴
- 管理者ユーザーでなくても使用できる
- インストール先は`/usr/local/Cellar/`で、`/usr/local/bin`にシンボリックリンクする（[[usrディレクトリ]]）

```shell
#パッケージ検索
$ brew search <package>

#install
$ brew install <package>

#uninstall
$ brew remove <package>

#formuls(手順書)の更新
$ brew update

#更新があるパッケージを再ビルド
$ brew upgrade

#インストールしたパッケージの表示
$ brew list

#インストールの問題をチェック
$ brew doctor
```