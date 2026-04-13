---
category: "[[Clippings]]"
author: "[[Yukii's Blog]]"
title: "MySQL で parent child なレコード群の bulk insert - Yukii's Blog"
source: https://blog.yukii.work/posts/2023-03-14-mysql-parent-child-bulk-insert/#gsc.tab=0
clipped: 2023-10-13
published: 
topics: 
tags: [clippings]
---

Published: 2023/3/14

---

### 問題

MySQL は、 PostgreSQL と違って (bulk) insert 文に対して、生成された id 群を返してくれない。 親子関係のあるレコード群を bulk insert していきたい場合に問題になるので、これはどうやって実現されているのか、調べてみた。

なお、ここでいう bulk insert とは、

```
INSERT INTO `users`(`name`, `age`) VALUES (('John', 28), ('Adam', 20));
```

のように一つの insert 文で複数レコードを挿入すること。

### 方法: LAST\_INSERT\_ID() を使う

MySQL には [`LAST_INSERT_ID()`](https://dev.mysql.com/doc/refman/8.0/ja/information-functions.html#function_last-insert-id) という関数があり、この関数は直前に実行された INSERT 文により生成された、最初の auto increment 値を返す。 これは接続ごとに保持される値であり、他の接続の影響を受けないため、同時実行の整合性を気にせずに利用できる。

[](https://stackoverflow.com/a/16592867/3090068)

また、上記のページによれば、 `innodb_autoinc_lock_mode` の値を適切に設定することにより、 bulk insert の際に生成される auto increment の id たちを連番にできる、とのこと。

この二つを組み合わせて、親(parent)のテーブルから insert し、 `LAST_INSERT_ID()` によって先頭のレコードの id を特定し、 `ROW_COUNT()` で更新された行数を取得することで、実際に insert されたレコードたちの id を計算できる。

---

###### Tags: [mysql](https://blog.yukii.work/tags/mysql)

### 関連記事