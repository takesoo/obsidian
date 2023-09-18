---
category: "[[Clippings]]"
author: "[[2021年6月26日]]"
title: "【Rails】in_batches、find_in_batches、find_eachの違いと使いどころ | Enjoy IT Life"
source: https://nishinatoshiharu.com/rails-roop-methods-difference/
clipped: 2023-08-29
published: 2021-06-26
topics: 
tags: [clippings, ruby/find_in_batches, ruby/find_each]
---

![](https://nishinatoshiharu.com/wp-content/uploads/2021/06/rails-roop-methods-difference.001.png) Ruby

`each`はデータをすべてメモリにのせてからループ処理を実行するため、大量のデータを扱う場合はメモリを圧迫させる可能性があります。

たとえば以下の例では、booksテーブルの全レコードがActive Recordオブジェクトに変換され、全オブジェクトをまとめた配列がメモリへ配置されることになります。

```

Book.all.each do |book|
  
end
```

**Railsで大量のデータをループ処理する場合、データを割してメモリにのせるin\_batches・find\_in\_batches・find\_eachを利用することでメモリ消費が抑えられます。**  
今回は`in_batches`・`find_in_batches`・`find_each`の違いについて紹介します。

## メソッドの概要と挙動について

`in_batches`、`find_in_batches`、`find_each`について紹介します。

### in\_batchesについて

[in\_batches](https://railsdoc.com/page/in_batches)はデータを分割して取得し、ActiveRecord::Relationオブジェクトの形でブロックへ渡します。デフォルトの分割単位は1,000件です。Rails 5.0.0.1から利用可能です。

`in_batches`のオプションは以下の通りです。[1](#fn-8217-in_batches-option)

オプション|内容
-|-
:of|バッチ数。デフォルトは1,000
:load|ロードの有無。デフォルトはfalse
:start|開始位置
:finish|終了キー
:error\_on\_ignore|例外を発生させる

`in_batches`はActiveRecord::Relationをブロックに渡すので、**ブロック内でwhereによる絞り込みやupdate\_allによる一括更新を行いたい時に向いているメソッド**です。

たとえば『booksテーブルにある全レコードのpriceを100増やす』という処理を`in_batches`で実装すると以下のようになります。

ソースコード

```
Book.in_batches do |relation|
  relation.update_all("price = price + 100")
end
```

update\_allはupdateとは違い、バリデーションとコールバックが実行されずレコードのupdate\_atも更新されません。  
update\_allとupdateの挙動は等価ではないので、『each + update』の代わりに『in\_batches + update\_all』を利用する際は注意が必要です。

ログは以下の通りです。  
発行されたSQLを見てわかる通り、`in_batches`を利用すると1,000件ずつデータが一括更新されます。

実行ログ

```
(1.0ms)  SELECT `books`.`id` FROM `books` ORDER BY `books`.`id` ASC LIMIT 1000
Book Update All (14.9ms)  UPDATE `books` SET price = price + 100 WHERE `books`.`id` IN (1, 2, 3, 4, 5,...

(0.8ms)  SELECT `books`.`id` FROM `books` WHERE `books`.`id` > 1000 ORDER BY `books`.`id` ASC LIMIT 1000
Book Update All (14.6ms)  UPDATE `books` SET price = price + 100 WHERE `books`.`id` IN (1001, 1002, 1003, 1004, 1005,...

(0.8ms)  SELECT `books`.`id` FROM `books` WHERE `books`.`id` > 2000 ORDER BY `books`.`id` ASC LIMIT 1000
Book Update All (14.6ms)  UPDATE `books` SET price = price + 100 WHERE `books`.`id` IN (2001, 2002, 2003, 2004, 2005,...

(0.6ms)  SELECT `books`.`id` FROM `books` WHERE `books`.`id` > 3000 ORDER BY `books`.`id` ASC LIMIT 1000
Book Update All (14.6ms)  UPDATE `books` SET price = price + 100 WHERE `books`.`id` IN (3001, 3002, 3003, 3004, 3005,...
...
...
```

### find\_in\_batchesについて

[find\_in\_batches](https://railsdoc.com/page/find_in_batches)はデータを分割して取得し、Arrayの形でブロックへ渡します。デフォルトの分割単位は1,000件です。

[find\_in\_batchesの実装](https://github.com/rails/rails/blob/f33d52c95217212cbacc8d5e44b5a8e3cdc6f5b3/activerecord/lib/active_record/relation/batches.rb#L126)を確認するとメソッドの中で`in_batches`を呼んでいることがわかります。  
つまり、**in\_batchesで渡されたオブジェクトをArrayに変換するメソッドがfind\_in\_batchesの正体**です。

`find_in_batches`のオプションは以下の通りです。[2](#fn-8217-find_in_batches-option)

オプション|内容
-|-
:batch\_size|バッチ数。デフォルトは1,000
:start|開始位置
:finish|終了キー
:error\_on\_ignore|例外を発生させる

たとえば『booksテーブルにある全レコードのpriceを100増やす』という処理を`find_in_batches`で実装すると以下のようになります。

ソースコード

```
Book.find_in_batches do |books|
  books.each do |book|
    book.price += 100
    book.save
  end
end
```

ログは以下の通りです。  
発行されたSQLを見てわかる通り、`find_in_batches`では1,000件ずつデータを取得しています。

実行ログ

```
Book Load (2.6ms)  SELECT `books`.* FROM `books` ORDER BY `books`.`id` ASC LIMIT 1000
  (0.9ms)  BEGIN
Book Update (10.9ms)  UPDATE `books` SET `books`.`price` = 880, `books`.`updated_at` = '2021-06-25 08:10:21.970912' WHERE `books`.`id` = 1
  (10.0ms)  COMMIT
  (0.3ms)  BEGIN
Book Update (0.7ms)  UPDATE `books` SET `books`.`price` = 880, `books`.`updated_at` = '2021-06-25 08:10:22.001425' WHERE `books`.`id` = 2
  (3.0ms)  COMMIT
  (0.4ms)  BEGIN
Book Update (0.6ms)  UPDATE `books` SET `books`.`price` = 740, `books`.`updated_at` = '2021-06-25 08:10:22.008483' WHERE `books`.`id` = 3
  (2.4ms)  COMMIT
  (0.3ms)  BEGIN
  ...
  ...
  ...
```

### find\_eachについて

[find\_each](https://railsdoc.com/page/find_each)はデータを分割して取得し、1件ずつActive Recordの形でブロックへ渡します。

[find\_eachの実装](https://github.com/rails/rails/blob/f33d52c95217212cbacc8d5e44b5a8e3cdc6f5b3/activerecord/lib/active_record/relation/batches.rb#L67)を確認するとメソッドの中で`find_in_batches`を呼び、`find_in_batches`によってブロックに渡されたArrayを`each`でループしていることがわかります。  
つまり、**find\_in\_batchesで渡されたArrayをeachでループするメソッドがfind\_eachの正体**です。

`find_each`は`each`と同様に1件ずつActive Recordをブロックに渡すので、**eachのループ処理を省メモリで行いたいときに向いているメソッド**です。

`find_each`のオプションは以下の通りです。[3](#fn-8217-find_each-option)

オプション|内容
-|-
:batch\_size|バッチ数。デフォルトは1,000
:start|開始位置
:finish|終了キー
:error\_on\_ignore|例外を発生させる

たとえば『booksテーブルにある全レコードのpriceを100増やす』という処理を`find_each`で実装すると以下のようになります。

ソースコード

```
Book.find_each do |book|
  book.price += 100
  book.save
end
```

ログは以下の通りです。  
発行されたSQLを見てわかる通り、`find_each`では1,000件単位でデータを取得したあと1件ずつブロックに渡しています。

実行ログ

```
Book Load (1.6ms)  SELECT `books`.* FROM `books` ORDER BY `books`.`id` ASC LIMIT 1000
  (0.5ms)  BEGIN
Book Update (0.6ms)  UPDATE `books` SET `books`.`price` = 880, `books`.`updated_at` = '2021-06-25 08:14:01.058354' WHERE `books`.`id` = 1
  (2.9ms)  COMMIT
  (0.3ms)  BEGIN
Book Update (0.6ms)  UPDATE `books` SET `books`.`price` = 880, `books`.`updated_at` = '2021-06-25 08:14:01.066300' WHERE `books`.`id` = 2
  (2.4ms)  COMMIT
  (0.2ms)  BEGIN
  ...
  ...
  ...
```

## データのソート順はバッチ処理に適用されないので注意

[in\_batchesの実装](https://github.com/rails/rails/blob/406d3b190044e21b4efc45c80d2e171595615cae/activerecord/lib/active_record/relation/batches.rb#L215-L223)を確認すると`primary_key`を利用してデータを取得していることがわかります。  
そのため、ソート処理をしたデータに対してin\_batchesを実行しても事前に行ったソート順は適用されません。

ソースコード

```

Book.order("price DESC").in_batches(of: 10) do | relation |
  p relation.map(&:price)
end
```

ログは以下の通りです。  
バッチ単位でみるとprice順にソートされていますが、ループ処理はid順になっています。  
また、`Scoped order is ignored, it's forced to be batch order.`というログからもソートが適用されていないことがわかります。

実行ログ

```
Scoped order is ignored, it's forced to be batch order.

   (0.7ms)  SELECT `books`.`id` FROM `books` ORDER BY `books`.`id` ASC LIMIT 10
  Book Load (0.6ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` IN (1, 2, 3, 4, 5, 6, 7, 8, 9, 10) ORDER BY price DESC
[1080, 1080, 1040, 1020, 1020, 800, 780, 780, 640, 640]

   (0.5ms)  SELECT `books`.`id` FROM `books` WHERE `books`.`id` > 10 ORDER BY `books`.`id` ASC LIMIT 10
  Book Load (0.5ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` IN (11, 12, 13, 14, 15, 16, 17, 18, 19, 20) ORDER BY price DESC
[940, 940, 900, 870, 830, 800, 740, 660, 620, 600]

   (0.5ms)  SELECT `books`.`id` FROM `books` WHERE `books`.`id` > 20 ORDER BY `books`.`id` ASC LIMIT 10
  Book Load (0.5ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` IN (21, 22, 23, 24, 25, 26, 27, 28, 29, 30) ORDER BY price DESC
[1050, 1000, 940, 910, 850, 760, 730, 720, 680, 600]
```

find\_in\_batchesとfind\_eachもin\_batches\`同様、事前に行ったソート順は適用されません。

参考として以下に`find_in_batches`と`find_each`を利用したケースも掲載しておきます。

ソースコード(find\_in\_batches)

```

Book.order("price DESC").find_in_batches(batch_size: 10) do |books|
  p books.map(&:price)
end
```

実行ログ(find\_in\_batches)

```
Scoped order is ignored, it's forced to be batch order.

  Book Load (0.8ms)  SELECT `books`.* FROM `books` ORDER BY `books`.`id` ASC LIMIT 10
[780, 780, 640, 1080, 1080, 1020, 1040, 800, 1020, 640]

  Book Load (0.6ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` > 10 ORDER BY `books`.`id` ASC LIMIT 10
[660, 940, 800, 600, 830, 940, 740, 900, 620, 870]

  Book Load (0.6ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` > 20 ORDER BY `books`.`id` ASC LIMIT 10
[940, 600, 1000, 730, 850, 1050, 680, 720, 760, 910]
```

ソースコード(find\_each)

```

Book.order("price DESC").find_each(batch_size: 10) do |book|
  print "#{book.price}, "
end
```

実行ログ(find\_each)

```
Scoped order is ignored, it's forced to be batch order.

  Book Load (0.7ms)  SELECT `books`.* FROM `books` ORDER BY `books`.`id` ASC LIMIT 10
780, 780, 640, 1080, 1080, 1020, 1040, 800, 1020, 640,

  Book Load (0.7ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` > 10 ORDER BY `books`.`id` ASC LIMIT 10
660, 940, 800, 600, 830, 940, 740, 900, 620, 870,

  Book Load (0.7ms)  SELECT `books`.* FROM `books` WHERE `books`.`id` > 20 ORDER BY `books`.`id` ASC LIMIT 10
940, 600, 1000, 730, 850, 1050, 680, 720, 760, 910,
```

**実行順に意味のあるループ処理ではin\_batches、find\_in\_batches、find\_eachを利用しない**ようにしましょう。

## ループ処理の対象が数千程度であればeachメソッドで問題ない

バッチ数のデフォルトが1,000であることからわかる通り、ループ処理の対象が数千程度であれば`each`で問題ありません。[4](#fn-8217-railsguides-active_record_querying)

## まとめ

今回のまとめ

-   in\_batchesはActiveRecord::Relationを渡す
-   find\_in\_batchesはArrayを渡す
-   find\_eachはActive Recordを渡す
-   in\_batches・find\_in\_batches・find\_eachではデータのソートは無視される注意
-   数千程度のデータ数であればeachでループ処理をしても問題ない

Twitter([@nishina555](https://twitter.com/nishina555))やってます。フォローしてもらえるとうれしいです！

## 参考

-   [Railsドキュメント『in\_batches』](https://railsdoc.com/page/in_batches)
-   [Railsドキュメント『find\_in\_batches』](https://railsdoc.com/page/find_in_batches)
-   [Railsドキュメント『find\_each』](https://railsdoc.com/page/find_each)
-   [ActiveRecordの#all,#find\_each,#find\_in\_batches,#in\_batchesの違いについて](https://daido.hatenablog.jp/entry/2019/04/16/195647)
-   [Railsでパフォーマンスを低下させないために気をつけること](https://qiita.com/nikadon/items/9e6431f5a7b2b8798113)
-   [Railsで大量データを扱うときに気をつけていること](https://techblog.lclco.com/entry/2019/07/31/180000)
-   [Rails/ActiveRecord バッチ処理の最適化](https://blog.toshimaru.net/rails-batch-optimization/)