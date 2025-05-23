---
tags:
  - Linux
---
## 概要
- サーバーなどで広く利用されている[[OS]]
- 他のOSと同様、[[カーネル]]とさまざまなソフトウェアの組み合わせで構成される。
- 複数の[[Linux ディストリビューション]]がある
	- Ubuntu, RedHat, etc...
- 用途
	- サーバー
	- デスクトップやワークステーション
	- スマートフォンや家電製品
	- [[組み込み機器]]
	- [[AI]]・[[機械学習]]
- 操作
	- 単発のコマンド実行
		- `cd`, `chmod`, `mkdir`, etc...
	- [[デーモンプロセス]]の起動・停止
		- `systemctl`
	- その他
		- [[Ctrl+d]]
		- [[Ctrl+c]]
		- [[Ctrl+z]]
## Linuxコマンド
- コマンドの実態はプログラムであり、[[/bin]]や[[/sbin]]といったプログラム用のディレクトリに配置されている。
- [[標準入出力]]がある
- [[パイプ]]で繋げることができる
- `&`をつけて実行するとバックグラウンドジョブとして実行される(`tail -f /var/log/dnf.log &`)
- コマンドの前に`HOGE=ture`と書くと、そのコマンドの実行時のみ一時的に[[環境変数]]を設定できる
```dataview
Table overview
FROM #Linux/コマンド 
SORT file.name
```
## ユーザーとグループ
- ユーザーは必ずひとつのグループに所属し（プライマリグループ）、さらに複数のサブグループに所属することができる
- ユーザーの定義は`/etc/passwd`に記述されている
- パスワードは`/etc/shadow`に記述されている
- グループの定義は`/etc/group`に記述されている
### root
- すべてのファイルに対するアクセスができる
- システム管理用のコマンドが実行できる
- 「特権ユーザー」「スーパーユーザー」
- [[su]]でrootに切り替えるか、[[sudo]]でroot権限でコマンド実行する
- セキュリティ保護の観点から、rootのパスワードを設定せず、suで切り替えられないように設定することが多くなっている
### wheelグループ
- [[sudo]]を実行できるグループ
- wheel = 船の操舵輪 
## アクセス制御
- ファイルやディレクトリには所有者と所有グループが設定される
	- ファイルを作成すると、作成したユーザーが所有者に、ユーザーのプライマリグループが所有グループになる
- ファイルのアクセス権には「読み込み(Read)」「書き込み(Write)」「実行(eXecute)」の3種類がある
- このアクセス権を、所有者、所有グループ、その他のユーザーの3つに対してそれぞれ設定できる
- アクセス権の確認は[[ls]]コマンドの`-l`オプション。変更は[[chmod]]。
## ディレクトリとファイル
### /etc
- 慣例として、設定ファイルが置いてあることが多いディレクトリ
#### /etc/passwd
- ユーザーの定義が記述されたファイル
- エディタで直接編集せず、[[useradd]]コマンドなどで操作する。
```bash
ユーザー名:パスワード:UID:GID:コメント:ホームディレクトリ:ログインシェル

# ユーザー名          ユーザー名
# パスワード          マスキングされており、Xと表示される
# UID               ユーザーID
# GID               ユーザーが属するプライマリグループのID
# コメント            ユーザー名またはコメントフィールド
# ホームディレクトリ   ユーザーのホームディレクトリ
# ログインシェル       ユーザーのログイン時に起動するコマンドインタプリタ
```
#### /etc/shadow
- ユーザーのパスワードが記述されたファイル
- エディタで直接編集せず、[[passwd]]コマンドで操作する
```bash
ユーザー名:パスワード:最終変更日:変更可能日:変更要求日:警告日数:無効日数:失効日数:予約

# ユーザー名    ユーザー名
# パスワード    暗号化されたパスワード
# 最終変更日    1970 年 1 月 1 日から、最後にパスワードが変更された日までの日数
# 変更可能日数  パスワードが変更可能となるまでの日数
# 変更要求日数  パスワードを変更しなくてはならなくなる日までの日数
# 警告日数      パスワード有効期限が来る前に、ユーザーが警告を受ける日数
# 無効日数      パスワード有効期限が過ぎ、アカウントが使用不能になるまでの日数
# 失効日数      1970 年 1 月 1 日からアカウントが使用不能になる日までの日数
# 予約         予約フィールド
```
#### /etc/group
- グループの定義が記述されたファイル
- エディタで直接編集せず、[[groupadd]]コマンドなどで操作する
```bash
グループ名:パスワード:GID:ユーザー

# グループ名  グループの名前
# パスワード  以前は暗号化されたグループのパスワード、またはパスワードが不要なら空欄
# GID       グループID番号
# ユーザー    グループに所属するユーザー名のリスト。それぞれのユーザー名はコンマで区切られる。
```
#### /etc/sudoers
- sudoコマンドの設定ファイル
- エディタで直接編集せず、[[visudo]]コマンドで変更する
#### /etc/resolv.conf
- [[Domain Name System|DNS]]名前解決設定ファイル
- DNSサーバーのIPアドレスを記載されている
- 設定の変更は[[Network Manager]]で行う
### /var
- ==var==iable
- システム運用中に生成されて削除されるデータを 一時的に保存するためのディレクトリ
- その他すべての多用途に使用できるファイルが保存され、ファイルのサイズが後ほど拡張し続ける場合、より適合