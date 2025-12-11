---
category: "[[Clippings]]"
author:
title: Railsでデータの件数を取得する方法を解説！count, size, lengthの違いは？
source: https://himakuro.com/rails-count-length-size-guide
clipped: 2023-10-10
published: 2021-01-17
tags:
  - clippings
---

![](https://himakuro.com/wp-content/uploads/2020/02/boy-nayamu.png)

RailsのActiveRecordでレコードの件数を調べるときにcount, size, lengthと、件数を取得するメソッドが3種類もあって、どれを使ったら良いのかわかりません。こういうときにはこれを使うべきというのがあれば教えて下さい！

こんな悩みを解決していきます。

**本記事の内容**

-   count ,size, lengthの違いを解説
-   悩んだ時はこれを使うべきというのがわかるようになる

RailsにはActive Recordという、データベースのデータを簡単に取得出来るようにしてくれる機能が搭載されています。

今回はそのActive Recordのメソッドで、データの件数を取得するための下記の3つについて見て行きます。

これらのメソッドはどれも結果としては件数が取得出来ますが、内部で行われている処理には違いがあります。

今回の記事を読む事でこの3つのメソッドの内部的な処理の違いを理解し、データベースにも優しい処理を書けるようになりましょう。

## countは必ずクエリを発行する

今回の記事ではRails Consoleを活用して、Usersテーブルのレコードを全件取得した後に各メソッドを実行したらどの様に処理されるかを見ていきます。

まずはcountメソッドの挙動です。

ここで注目して欲しいのはcountメソッドを実行すると毎回データベースに対してクエリを投げているという点です。

基本無いとは思いますが、eachなどのループ処理の中でcountを実行する際にな十分注意しましょう。

countメソッドの特徴

-   実行時に必ずデータベースに対してクエリを発行する
-   毎回クエリを発行するため、必ず最新のレコードの件数を取得出来る

[RailsのcountメソッドのコードをGithubで見る場合はこちら](https://github.com/rails/rails/blob/8e3b1a632c15e8c7f708ab222a81faaad8e3410e/activerecord/lib/active_record/associations/collection_association.rb#L220)

## lengthはメモリに情報をロード(キャッシュ)する

lengthは1つめのcountと違い実行時にメモリに情報を保存します。

そのため、2回目以降のlengthではSELECT `users`.\* FROM `users`が実行(表示)されていない点に注目しましょう。

lengthメソッドの特徴

-   初回のlength実行時にデータをメモリに保存している
-   一度データをロードした後は何度lengthを実行しても新たなクエリは発行されない

[RailsのlengthメソッドのコードをGithubで見る場合はこちら](https://github.com/rails/rails/blob/8e3b1a632c15e8c7f708ab222a81faaad8e3410e/activerecord/lib/active_record/associations/collection_association.rb#L301)

## sizeはlengthとcountのあわせ技

sizeメソッドはデータをallで取得した後に実行するとcountと同様に必ずクエリを発行します。

しかし下記の様に間にlengthを挟みメモリ上にデータを乗せておくと、sizeもlengthと同様にメモリから取得するようになりクエリを発行しなくなります。

sizeメソッドの特徴

-   メモリにデータが無い場合はCountと同じくクエリを発行する
-   メモリにデータがある場合はlengthと同じくクエリを発行せずに件数を取得可能

[RailsのsizeメソッドのコードをGithubで見る場合はこちら](https://github.com/rails/rails/blob/8e3b1a632c15e8c7f708ab222a81faaad8e3410e/activerecord/lib/active_record/associations/collection_association.rb#L279)

## loadメソッドでメモリにデータを格納する

![](https://himakuro.com/wp-content/uploads/2020/02/boy-programming.png)

lengthを使えばメモリ上にデータが乗るからクエリが発行しなくなるのは分かったけど、lengthを使ってメモリに乗せて、その次からはcountを実行するのはなんか微妙な気がする…

こういう風に思った方もいると思います。

そんな時はloadメソッドを活用してメモリ上にデータを乗せてからsizeを使う方法があります。

loadメソッドを使った後はlengthの時と同様に、sizeで件数を取得してもクエリが発行されないようになります。

## 件数を取得するだけならメソッドチェーン

今までのコードでは一度usersという変数にallの結果を入れてからcountなどの件数を取得するメソッドを実行していました。

しかしレコードの内容は不要で件数だけが必要な場合は、メソッドチェーンを使うことでよりシンプルに書けます。

## 件数を取得する場合はどのメソッドを使うべきか

基本的にはlengthとcountの合せ技のsizeを使うのが安定ですが、1つだけ注意点があります。

それはデータがメモリ上に乗っている（キャッシュされている）場合は最新の件数を取得出来ない点です。

countは必ずクエリを発行するため、処理の途中でデータがインサート（作成）された場合でも常に最新の件数を取得することが出来ます。

一方sizeはデータがキャッシュされている場合は、クエリを発行せずにキャッシュから件数を取得するために必ずしも正確な件数を取得出来ない場合があります。

何も考えずにsizeメソッドを使った場合にはこの様な罠があるという事を覚えておきましょう。

## メソッドは理解した上で使うべき

似たような処理を行うメソッドでも内部の処理は大きく異なります。

具体的にどのような処理が行われているかを意識しその時の最適なメソッドを選択する事で、パフォーマンスを意識したより良い実装を出来るようになりましょう。