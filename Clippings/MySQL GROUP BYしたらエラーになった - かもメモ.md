---
category: "[[Clippings]]"
author: "[[かもメモ]]"
title: MySQL GROUP BYしたらエラーになった - かもメモ
source: https://chaika.hatenablog.com/entry/2019/02/19/120000
clipped: 2023-09-26
published: 2019-02-19
topics: 
tags:
  - clippings
  - mysql
---

お久しぶりの[MySQL](http://d.hatena.ne.jp/keyword/MySQL)。  
[MySQL](http://d.hatena.ne.jp/keyword/MySQL)で重複してるデータを取ろうとしました。

SELECT \* FROM table\_a
GROUP BY name
HAVING COUNT(\*) >= 2;

`Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'test.table_a.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by`

[MySQL](http://d.hatena.ne.jp/keyword/MySQL) 5.7からデフォルトで`ONLY_FULL_GROUP_BY`オプションがONになっており、GROUP BYするデータだけを取ってきなさいよ。という事でGROUP BYで使っているカラム以外もSELECTしているとエラーになるというもののようです。([MySQL](http://d.hatena.ne.jp/keyword/MySQL) 5.7から...ずいぶん[SQL](http://d.hatena.ne.jp/keyword/SQL)文かいてなかったのがバレますﾈ...)

一部のカラムでGROUP BYして、全部のデータを取ってきたいなら、GROUP BYしたデータサブクエリにしてWHEREの条件にすればOKっぽいです。

SELECT \* FROM table\_a
WHERE name in (
  SELECT name FROM table\_a
  GROUP BY name
  HAVING COUNT(\*) >= 2
);

[SQL](http://d.hatena.ne.jp/keyword/SQL)文長くなりましたが、`name`カラムが重複してるレコードのカラム全部を表示することができました！  
`ONLY_FULL_GROUP_BY`オプションをOFFにする方法もあるようですが、何かしら理由あっての事だと思うので慣れたいと思います。

---

\[参考\]

-   [MySQLでGROUP BY句を使ったSQL文がエラーになる原因と対応 | jMatsuzaki](https://jmatsuzaki.com/archives/18846)
-   [重複しているレコードを検索するSQL(大量データも対応) - Qiita](https://qiita.com/necoyama3/items/4c24defd6f504366aebe)

[![これからはじめる MySQL入門](https://images-fe.ssl-images-amazon.com/images/I/51xZAvjwB2L._SL160_.jpg "これからはじめる MySQL入門")](http://www.amazon.co.jp/exec/obidos/ASIN/4774197599/kikiki83-22/)