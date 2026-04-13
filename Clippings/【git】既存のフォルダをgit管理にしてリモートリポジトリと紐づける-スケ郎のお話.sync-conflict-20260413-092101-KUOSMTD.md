---
Created: Invalid date
URL: https://www.sukerou.com/2019/12/git-tipsgit.html
---
既存のフォルダを、git管理下にしてリモートリポジトリと紐づけする方法を解説します。

物（ソース）を先に作成してしまってから、後からgitに登録したいなと思った時に、便利な方法です。

スポンサーリンク

## 手順１：フォルダをgit管理にする

ターミナルを開いて、対象のフォルダに移動後、`git init`コマンドを実行して、フォルダをgit管理下にします。

```
cd /path/to/dirgit init
```

## 手順２：リモートリポジトリの設定

git管理化にしたフォルダに、リモートリポジトリを設定します。

・ SSH版（GitHub）

```
$ git remote add origin git@github.com:ユーザ名/リポジトリ名.git
```

・ SSH版（HTTPS）

```
$ git remote add origin https://github.com/ユーザ名/リポジトリ名.git
```

スポンサーリンク

## 手順３：gitにファイルを追加

フォルダ内のすべてのファイルを、Git管理するために`git add`でステージングします。git管理外にしたいファイルがある場合は、個別に`git add`してください。

次に、`git commit`でローカルのgitリポジトリをコミットします

```
git add *git commit -m 'first Commit.'
```

## 手順４：リモートリポジトリにプッシュ（git push)

最後に、`git push`でリモートリポジトリにプッシュします。

```
git push origin master
```

ここまでの手順で、git管理外の既存のフォルダを、git管理化してリモートへファイルをプッシュできます。

スポンサーリンク

## その他情報

### コミット時のユーザ名、メールアドレスを設定する。

▪️ グローバル設定

```
※ 全体の設定をしたいとき$ git config --global user.name "Yamada Tarou"$ git config --global user.email "yamada@example.com"
```

▪️ リポジトリ毎の設定

```
※ リポジトリ（ディレクトリ）ごとにauthorを登録したいとき$ git config user.name "Yamada Tarou"$ git config user.email "yamada@example.com"
```

### macでgitコマンドをたたくとエラーになる

macOSをアップデートすると、gitコマンド入力時に、次のようなエラーが発生することが報告されています。

```
$ git --versionxcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

そのような時は、次のコマンドでxcodeをインストールすれば解決します。

```
xcode-select --install
```

## さいごに

既存のフォルダを、git管理下にする方法を紹介してきました。  
なんとなく作ったソースを、後からGitHubに登録したくなった場合などに便利です。

### 0 件のコメント: