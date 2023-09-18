[https://qiita.com/ug23/items/308182bf4e60bc81f04e](https://qiita.com/ug23/items/308182bf4e60bc81f04e)

  

More than 3 years have passed since last update.

[Fringe81 Advent Calendar 2017](https://qiita.com/advent-calendar/2017/fringe81)Day 13

これは[Fringe81 Advent Calendar 2017](https://qiita.com/advent-calendar/2017/fringe81)の13日目の記事です。

こんにちは。 [@ug23](https://qiita.com/ug23) です。

今回はサーバからのアラートや調査のためにサーバにログインしてログを調査するときに便利なコマンド・小技を紹介します。  
何度も教えまくっていていちいち説明するのも面倒だなと思い始めてきたのでまとめてみました。  
もっといい方法や誤っている部分、危険だよということがあればぜひコメントや編集リクエストをいただけると嬉しいです。

※基本的にBashのお話です。

## TL;DR

- ログファイル確認と監視は`less`。ショートカットキーは色々あります。
- 検索はやっぱり `grep`。 オプションや正規表現を覚えるとより幸せになれます。
- 集計するときは`awk`。一度パターンを作ってしまえば自分の武器になります。
- Bashのワイルドカードは の他にもあります。

言わずと知れたファイル監視コマンドです。監視の際は`-F` を付けましょう

```
# 名前が変わろうとも同じファイルを監視する => ログローテーションされたら開き直す必要ありtail -f access.log# 名前が変わると自動で開き直す => 時間や日付が変わってもそのまま見ていられるtail -F access.log# ワイルドカード指定しておくとファイルごとの更新分をまとめて見れるtail -F *.log
```

`grep` や `awk` に流すこともできますが、そんなときはバッファされないオプションを付けておきましょう。[bashでパイプとかでつないでバッファされちゃう時](https://gist.github.com/riywo/874011)

`view` や `more` より高機能で高速なファイル確認コマンドです。`cat` ではターミナルを遡らなければなりませんが、 `less` ならコマンド内でスクロールできます。  
また、ログ確認で `vi` `emacs` `nano`などエディタを開いてしまうと改変、削除の危険性があるのでやめましょう。

代表的なオプションやコマンドを紹介しておきます。

|+F|ログ監視モードになる（ctrl+cで普通のlessに戻る）|
|---|---|
|[[--follow-name]]|開いている途中にファイル名が変更されたらファイルを開き直す|
|[[-R]]|ANSIエスケープシーケンスを解釈して色が付く|
|[[-S]]|行を折り返さずに表示する|
|[[-X]]|lessを閉じても画面をクリアしない|

  
  

|q|閉じる|
|---|---|
|[[F]]|ログ監視モードになる|
|[[スペース または f]]|1ページ進む|
|[[b]]|1ページ戻る|
|[[G]]|一番下へ|
|[[G]]|一番上へ|
|[[word 2]]|_word_ という単語を下方向に検索(大文字小文字を区別する)|
|[[word]]|_word_ という単語を上方向に検索(大文字小文字を区別する)|
|[[n]]|最後に検索した単語をもう一度下方向に検索|
|[[n]]|最後に検索した単語をもう一度上方向に検索|
|[[-n]]|**次のファイルへ** (後述)|
|[[-p]]|**前のファイルへ** (後述)|

  
  

上記でファイル移動ができる旨を書きましたが、 `less` もファイル名をワイルドカード指定できます。

このとき最初は `fuga.txt` が読まれますが `:n` すると `hoge.txt` を開けます。  
ワイルドカードの昇順に切り替えることができます。

私の手元の環境では `less` 起動中に `h` を押すとすべてのショートカットキーが出ました。ぜひ覚えてみてください。

ファイル確認のみなら `less` が優秀ですが、`cat` は `concatenate` の略なので**ファイルを連結**することができます。

たとえば `access.log.YYYY-MM-DD_HH` という形式のログが置かれているとしましょう。

```
# 2017-12-01 分の全ログをまとめてgrepできるcat /path/to/access.log.2017-12-01_* | grep "ERROR"# もちろんlessだってできるcat /path/to/access.log.2017-12-01_* | less
```

検索といえばgrepですね。検索キーワードが含まれている行のみ出力してくれます。

```
# "Exception" という文字列を検索する（大文字小文字を区別します）cat error.log | grep Exception# 大文字小文字を区別したくないときcat error.log | grep -i error# "INFO" 以外の行を出力するcat application.log | grep -v INFO# AND検索cat access.log | grep 500 | grep /a# OR検索cat application.log | grep -e WARN -e ERROR# どのファイルにそのキーワードが含まれているかがわかるgrep "ERROR" *.log
```

私がよくつかうオプションを紹介しておきます。

|-c|該当する行数を検索できる|
|---|---|
|[[-i]]|case-insensitiveになる|
|[[-v]]|キーワードを含まない行だけ出力|
|[[-B X]]|該当する行のX行前から出力|

  
  

これだけでいろんなプログラムをかけてしまう `awk` です。アクセスログなどの集計時等に力を発揮します。基本的な使い方は他の記事に譲るとして、実際に私が使っているのを紹介します。

アクセスログがタブ区切りになっていればawk側もタブ区切りで読み込むようにしてやれば自由自在です。

いざという時に使えないと困るので時間のある時、安全な場所で以下のようにして一度カラムと内容の対応付けをしておくとよいです。他によいやり方があるかもしれませんが、こうしておけば確実にわかりますｗ

```
# 1列目cat access.log | awk -F"\t" "{print $1}"# 2列目cat access.log | awk -F"\t" "{print $2}"# 3列目…cat access.log | awk -F"\t" "{print $3}"
```

ちなみにcatを噛ませなくてもawk自身でファイルを読み込むことはできますが、順番を忘れることが多いのでcatからパイプで繋いでいます。

```
# こっちだっけ？awk filename "{print $5}"# それともこっちだっけ？（こっちが正解）awk "{print $5}" filename# grepはこっちが正解grep "ERROR" filename
```

例えば。  
あるサーバの `POST /hoge` というリクエストが特定の時間だけ `50X` を返してしまうということがありました。  
そのサーバは複数のエンドポイントを持っているので他のリクエストを除いてこのエンドポイントのみの情報を見たいという時以下のようにしました。

```
# $3: timestamp $4: リクエスト $5: レスポンスコードcat access.log | awk -F"\t" '$4 == "POST /hoge HTTP/1.1"{print substr($3,13,8),$5}'# => HH:MM:SS <レスポンスコード> というように出力できた
```

条件の指定の仕方や組み込み関数を使って自分だけの集計コマンドを作って用意しておけばいざという時も安心です。

### おまけ: Bashのワイルドカード

たとえば `ls *.log` のような指定は皆さんよく使うと思いますが、以下のような指定の仕方もできます。主にファイル名指定に使えます。

|?|任意の一文字|
|---|---|
|[[]]|任意の文字列|
|[[[0-9]]]|0から9の数字1文字|
|[[[!0-9]]]|0から9の数字 **以外の1文字**|
|[[[0-9!]]]|0から9の数字と **!**のどれか|
|[[[abc]]]|aかbかc, [a-c]と同義|
|[[[a-zA-Z]]]|大文字小文字を問わない半角英字1文字|
|[[{access,error}]]|"access"と"error"という文字列。スペースも認識される|

  
  

この辺を上手く使うと幸せになれました。

Why not register and get more from Qiita?

1. We will deliver articles that match youBy following users and tags, you can catch up information on technical fields that you are interested in as a whole
2. you can read useful information later efficientlyBy "stocking" the articles you like, you can search right away

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fug23%2Fitems%2F308182bf4e60bc81f04e&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fug23%2Fitems%2F308182bf4e60bc81f04e&realm=qiita)