---
category: "[[Clippings]]"
author: "[[by vintersnow]]"
title: homebrewとは何者か。仕組みについて調べてみた - Qiita
source: https://qiita.com/omega999/items/6f65217b81ad3fffe7e6
clipped: 2023-10-02
published: 2014-12-04
tags:
  - clippings Mac homebrew
---

## [](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)はじめに

いつも使っている`Mac book`。  
パッケージマネージャーは`brew`を使っています。

いつも何気なく使っている`brew`ですが、よくわかっていないのにネットの情報をコピペ→実行してしまうときがあります。  
今回はそんな`homebrew`についてちょっと調べてみました。

## [](#homebrew%E3%81%A8%E3%81%AF%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E3%81%A8%E3%81%AF)homebrewとは?パッケージ管理システムとは？

[wikipedia](http://ja.wikipedia.org/wiki/Homebrew_%28%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%29)によるとこうなっています。

**「Mac OS Xオペレーティングシステム上でソフトウェアの導入を単純化するパッケージ管理システムのひとつである」**

実行ファイルや設定ファイル、ライブラリetcを一つのファイルとしてまとめているものをパッケージと呼びます。  
パッケージ管理システムとはこのパッケージのインストール（アンインストール）作業を一元的管理するものです。パッケージやライブラリの依存関係などが管理できます。  
ちなみにパッケージ管理には、

1.  バイナリを取得するもの
2.  ソースコードを取得してビルドするもの

のパターンがあるみたいです。  
1バイナリとはソースコードを予めビルドした成果物を配布していることを指すようです。  
なので1バイナリは2ビルドよりも早くインストールできる特徴を持っているようです。  
それに対して2ビルドは、自分の`mac`でビルドするので自分の`mac`に最適化出来る特徴を持っているようです。

`mac`を使う方であれば`macports`や`fink`を使っている場合もあるかと思います。少し前までは`macports`が人気だったりしたようですね。

## [](#%E4%BB%96%E3%81%AE%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E3%81%A8%E3%81%AE%E9%81%95%E3%81%84%E3%81%AF)他のパッケージ管理システムとの違いは？

`homebrew`と`macports`の比較をされている記事があったので紹介させて頂きたいと思います。

　

　homebrew　

　macpors　

バイナリ/ビルド

基本ビルド

結構バイナリ

既存のソフトウェアへの影響

なるべく既存を利用

新しくインストールする

インストール可能なユーザ

一般ユーザ

スーパーユーザ(管理者権限)

パッケージのインストール先

/usr/local

/opt/usr

パッケージ数

少ない

多い

インストールにかかる時間

少ない

多い

システムへの依存度

大きい

小さい

コンパイラ

clang

clang

`macports`はバイナリがない場合にビルドするというハイブリット形式らしいです。`homebrew`では基本ビルドのようで。  
インストール時のユーザも`homebrew`では一般ユーザで可能ですが、`macports`では管理者権限がないと実行出来ないのですね。  
扱えるパッケージ数も`homebrew`の方が少ないようです。  
コンパイラは`clang`ですが、他のパッケージ管理システムでは`gcc`とかもあるようです。  
結果的に、どちらがいいのかという点についてですが、homebrew\`では依存関係でインストールされるソフトが少ないから人気が高まってきているのではないかと記事では考察されていました。パッケージ導入時のシステムへの負担や、インストールにかかる時間が比較的少なくて済むようです。  
とは言え、一長一短な気もするので使う方の好みであるとは思います。

ちなみに`homebrew`の場合パッケージは/usr/localの中のCellarというディレクトリにインストールされます。そしてインストール時にbinにリンクされるという仕組みになっているようです。まとめるとこんな感じ。

-   /usr/local/Cellar　コマンド実体
-   /usr/local/bin　　　コマンドのエイリアス

## [](#homebrew%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)homebrewについて

さて、本題の`homebrew`について紹介したいと思います。  
`homebrew`が何者かを知る上でですが、「`homebrew`」という名前そのものにヒントがあったことに今更気づきました。  
記事にあった表を引用して紹介します。

キーワード

本来の意味

たとえ

brew

ビールを醸造する

makeする

homebrew

自家醸造

ユーザ自らがビルドする

celler

ビール貯蔵庫

インストール（保存）先

keg

樽、醸成用

make材料

formula

調理法、手順

ビルド方法、手順が書かれたスクリプト

`brew`というのは「醸造する」という意味にあたるんですね。（今更）  
ということで、`homebrew`とは「ユーザが自らパッケージをビルドして使用する」ことのメタファーで「ビールを自家醸造して保存する・飲む」ことを意味しているのです。  
手順（調理法formula）通りにパッケージをビルド（醸造）して保存（/usr/local/cellerに格納)して、使う（/usr/loca/binにリンク）ってことのようです。  
そういうたとえあるとわかりやすいたちです自分。はい。

## [](#homebrew%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)homebrewのインストール

ここまで記事呼んで`homebrew`を使っていない方はいますでしょうか？  
下記コマンドでインストールしましょう。`homebrew`は`ruby`で書かれています。

bash

```
$ ruby -e "$(curl -fsS http://gist.github.com/raw/323731/install_homebrew.rb)” #homebrewのインストール
```

## [](#homebrew%E3%81%AE%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)homebrewの基本操作

基本操作を載せて置きたいと思いますので、困ったら確認してみましょう。

-   パッケージ検索

-   パッケージインストール・アンインストール

bash

```
$ brew install <パッケージ名>　#インストール
$ brew remove <パッケージ名>　#アンインストール
```

-   パッケージの有効化・無効化

bash

```
$ brew link <パッケージ名> #有効化
$ brew unlink <パッケージ名> #無効化
```

-   パッケージ一覧の更新

bash

```
$ brew update #formuls（手順書）の更新
$ brew upgrade #更新があるパッケージを再ビルド
```

-   パッケージされたリストの表示

-   デッドリンクになっているものを削除

-   インストールの問題をチェック

-   brewの設定確認

## [](#brew%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E4%B8%80%E8%A6%A7)brewコマンド一覧

`homebrew`の使い方一覧をまとめていた記事を見つけたので紹介させていただきたいと思います。

-   brew install foo fooをインストール
-   brew install --HEAD foo fooのHEADバージョンをインストール
-   brew install --force --HEAD foo fooの新しいHEADバージョンをインストール
-   brew search インストール可能なすべてのformulaを表示
-   brew search foo インストール可能なformulaからfooを検索
-   brew search /foo/ 正規表現fooを検索
-   brew list インストール済みのformulaeを表示
-   brew list foo fooのインストールしたファイルを表示
-   brew info --github foo foo formulaのGithub履歴ページをブラウザで表示
-   brew info インストール済みのHomebrewパッケージのサマリーを表示
-   brew info foo インストール済みのfooのすべての情報を表示
-   brew home HomebrewのWebサイトをデフォルトブラウザで表示
-   brew home foo fooのWebサイトをデフォルトブラウザで表示
-   brew update HomebrewのformulaeとHomebrew自体をアップデート
-   brew remove foo fooのアンインストール
-   brew create \[url\] ダウンロード可能なファイルのURLのformulaを生成してEDITORで指定されているエディタで開く
-   brew create url-of-tarball --cache formulaを生成して、tarballをダウンロードする。md5をformulaテンプレートに追加する。
-   brew create --macports foo どのようにfooをインストールするか調べるために、MacPortsパッケージ検索ページでfooを検索する。
-   brew create --fink foo Finkで同様のことを行う。
-   brew edit foo formulaをEDITORで開く
-   brew link foo fooのインストールされたファイルのHomebrew prefixシンボリックリンク作成する。（Homebrewでインストールすると自動的に行われる。DIYインストールを行った場合に有用。
-   brew unlink foo Homebrew prefixシンボリックリンクを削除する。
-   brew prune Homebrewprefixからデッドシンボリックリンクを削除する。
-   brew outdated 利用可能なアップデートバージョンが存在するformulaを表示する。新しいバージョンをインストールするにはbrew install fooを実行する。
-   brew upgrade 利用可能なアップデートバージョンが存在するformulaをすべてアップグレードする。
-   brew --config Homebrewのシステム設定を表示する
-   brew --prefix Howebrew prefixのパスを表示する。(普通は /usr/local)
-   brew --prefix (formula) インストールされたformulaのパスを表示する。
-   brew --cellar Homebrew Cellarのパスを表示する。(普通は /usr/local/Cellar)
-   brew --cache Homebrew キャッシュダウンロードのパスを表示する。(普通は ~/Library/Caches/Homebrew)
-   brew doctor インストールの一般的な問題をチェックする。
-   brew audit すべてのformulaeのコードとスタイルの問題を検査する。
-   brew cleanup foo インストールしたすべてもしくは特定のformulaeの古いバージョンをcellarから削除する。すべての場合はbrew cleanupを実行する。

## [](#%E3%81%8A%E3%82%8F%E3%82%8A%E3%81%AB)おわりに

`homebrew`について概要がわかっただけでもだいぶ進歩しました。パッケージ管理システムを全部比べたらなにか面白いことがわかるかもしれないのでどなたか比較検証よろしくお願いします（自分ではやらない）。ご参考にどうぞ。  
何か間違いがあればコメントして頂けると幸いです。

## [](#%E5%8F%82%E8%80%83%E8%A8%98%E4%BA%8B)参考記事

今回は多くの記事を参考に記事を描かせていただきました。ありがとうございました。

> homebrewの仕組みについてまとめておく  
> [http://takuya-1st.hatenablog.jp/entry/20111224/1324750111](http://takuya-1st.hatenablog.jp/entry/20111224/1324750111)  
> MacPortsとhomebrewの違いについて  
> [http://osanai.org/57/](http://osanai.org/57/)  
> MacPortsより使いやすい！？パッケージ管理システムhomebrewの使い方  
> [http://tukaikta.blog135.fc2.com/blog-entry-183.html](http://tukaikta.blog135.fc2.com/blog-entry-183.html)  
> パッケージ管理システムhomebrew  
> [http://qiita.com/b4b4r07/items/6efebc2f3d1cbbd393fc](http://qiita.com/b4b4r07/items/6efebc2f3d1cbbd393fc)  
> MacOSXのパッケージ管理システム  
> [http://www.slideshare.net/TomohikoHimura/ss-20115472](http://www.slideshare.net/TomohikoHimura/ss-20115472)