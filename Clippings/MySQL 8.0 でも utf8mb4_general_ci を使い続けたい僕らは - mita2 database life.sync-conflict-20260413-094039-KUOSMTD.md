---
category: "[[Clippings]]"
author: "[[mita2 database life]]"
title: "MySQL 8.0 でも utf8mb4_general_ci を使い続けたい僕らは - mita2 database life"
source: https://mita2db.hateblo.jp/entry/2020/12/07/000000
clipped: 2023-10-16
published: 2020-12-07
topics: 
tags: [clippings]
---

このエントリーは [MySQL Advent Calendar 2020](https://qiita.com/advent-calendar/2020/mysql) の 12/7 のエントリーです。

## 照合順序（COLLATION）とは

照合順序は文字列の比較やソート順のルールのことです。各キャ[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)[タセット](http://d.hatena.ne.jp/keyword/%A5%BF%A5%BB%A5%C3%A5%C8)ごとに照合順序が定義されています。

\-- SHOW COLLATIONS で一覧が見れる
mysql> SHOW COLLATIONS;
+----------------------------+----------+-----+---------+----------+---------+---------------+
| Collation                  | Charset  | Id  | Default | Compiled | Sortlen | Pad\_attribute |
+----------------------------+----------+-----+---------+----------+---------+---------------+
| armscii8\_bin               | armscii8 |  64 |         | Yes      |       1 | PAD SPACE     |
| armscii8\_general\_ci        | armscii8 |  32 | Yes     | Yes      |       1 | PAD SPACE     |
| ascii\_bin                  | ascii    |  65 |         | Yes      |       1 | PAD SPACE     |
| ascii\_general\_ci           | ascii    |  11 | Yes     | Yes      |       1 | PAD SPACE     |
| big5\_bin                   | big5     |  84 |         | Yes      |       1 | PAD SPACE     |
| big5\_chinese\_ci            | big5     |   1 | Yes     | Yes      |       1 | PAD SPACE     |
| binary                     | binary   |  63 | Yes     | Yes      |       1 | NO PAD        |
<snip>

[MySQL](http://d.hatena.ne.jp/keyword/MySQL) 8.0 で、utf8mb4 の照合順序が増えました。以下の表で太字にしたものが、新規に追加されたものです。各文字列が、同一と扱われる場合は、○としています。 `ci/cs` は `Case [In]sensitive` の略で、`as` は `Acent Sensiteve`、`ks` は `Katakana Sensitive` の略です。

COLLATION

A、a

はは、ぱぱ

はは、ハハ

びょういん、びよういん

🍣、🍺

＋、+

utf8mb4\_bin

×

×

×

×

×

×

**utf8mb4\_0900\_bin**

×

×

×

×

×

×

utf8mb4\_[unicode](http://d.hatena.ne.jp/keyword/unicode)\_ci

○

○

○

○

○

○

utf8mb4\_general\_ci

○

×

×

×

○

×

utf8mb4\_[unicode](http://d.hatena.ne.jp/keyword/unicode)\_520\_ci

○

○

○

○

×

○

**utf8mb4\_0900\_ai\_ci**

○

○

○

○

×

○

**utf8mb4\_0900\_as\_ci**

○

×

○

○

×

○

**utf8mb4\_ja\_0900\_as\_cs**

×

×

○

×

×

○

**utf8mb4\_ja\_0900\_as\_cs\_ks**

×

×

×

×

×

○

アルファベットの大文字・小文字を区別しない要件で、どれが選ばれそうか・・・ `utf8mb4_0900_as_ci` は「びょういん」「びよういん」が同一と扱われてしまい、いまいちに感じます。

そもそも、日本語の文字列比較やソート結果を網羅的に精査するのは現実的に可能なんでしょうか（上記の表以外にも考えないといけない、パターンがありそうです）。日本語には異字体・長音記号・漢数字・・・ちょっと思いつくだけでも、扱いに悩みそうな要素が多くあります。

絵文字が区別できないとは言え、`utf8mb4_general_ci` にはずっと使ってきた実績と安心があります。 [MySQL](http://d.hatena.ne.jp/keyword/MySQL) 8.0 でも `utf8mb4_general_ci` を 引き続き使うケースが多いのではないでしょうか。

## [MySQL](http://d.hatena.ne.jp/keyword/MySQL) 8.0 で utf8mb4\_general\_ci を使うときの注意点

### ALTER TABLE CONVERT TO 時に COLLATION の指定が必要

[MySQL](http://d.hatena.ne.jp/keyword/MySQL) 8.0 で utf8mb4 のデフォルトの照合順序が `utf8mb4_general_ci` から `utf8mb4_0900_as_ci` に変更になりました。

あわせて、従来の3バイトUTF8、utf8(mb3) は deprecated になっています。 utf8mb4 に変換するときに `COLLATE` を明示的に指定しないと、`utf8_general_ci` から `utf8mb4_0900_ai_ci` へとテーブルのデフォルト照合順序になってしまいます。

mysql> SELECT \* FROM utf8t WHERE c1 = "ぱぱ";
Empty set (0.00 sec)

mysql> ALTER TABLE utf8t CONVERT TO CHARACTER SET 'utf8mb4';
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

-- 「ぱぱ」で「はは」がヒットしてしまう
mysql> SELECT \* FROM utf8t WHERE c1 = "ぱぱ";
+----+--------+
| pk | c1     |
+----+--------+
|  1 | はは   |
+----+--------+
1 row in set (0.00 sec)

mysql> SHOW CREATE TABLE utf8t \\G
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 1. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
       Table: utf8t
Create Table: CREATE TABLE \`utf8t\` (
  \`pk\` bigint unsigned NOT NULL AUTO\_INCREMENT,
  \`c1\` varchar(255) DEFAULT NULL,
  UNIQUE KEY \`pk\` (\`pk\`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4\_0900\_ai\_ci
1 row in set (0.00 sec)

このように、`COLLATE` を指定してALTERする必要があります。

mysql> ALTER TABLE utf8t CONVERT TO CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4\_general\_ci';
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql>  SHOW CREATE TABLE utf8t \\G
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 1. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
       Table: utf8t
Create Table: CREATE TABLE \`utf8t\` (
  \`pk\` bigint unsigned NOT NULL AUTO\_INCREMENT,
  \`c1\` varchar(255) COLLATE utf8mb4\_general\_ci DEFAULT NULL,
  UNIQUE KEY \`pk\` (\`pk\`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4\_general\_ci
1 row in set (0.00 sec)

### SET NAMES では COLLATE の指定が必要

同様に、`SET NAMES` で照合順序を明示的に指定していない場合、[MySQL](http://d.hatena.ne.jp/keyword/MySQL) 8.0 からは `utf8mb4_0900_as_ci` が使われてしまいます。

\# MySQL 8.0 以降は utf8mb4\_0900\_as\_ci が使われる
mysql> SET NAMES utf8mb4;

# MySQL 8.0 以降は 明示的に utf8mb4\_general\_ci を指定する必要がある。
mysql> SET NAMES utf8mb4 COLLATE utf8mb4\_general\_ci;

## 過去の[MySQL](http://d.hatena.ne.jp/keyword/MySQL) Advent Calendar

気付けば、[MySQL](http://d.hatena.ne.jp/keyword/MySQL) Advent Calendar を 6年も続けてました・・・。時が経つのは早い・・・。 明日は、[lhfukamachi](https://twitter.com/lhfukamachi) さんです！

-   2015 [qiita.com](https://qiita.com/mita2/items/8fd915164f0851c96e54)
-   2016 [qiita.com](https://qiita.com/mita2/items/e8dd3787f9c165d53536)
-   2017 [mita2db.hateblo.jp](https://mita2db.hateblo.jp/entry/mysql-deadlock)
-   2018 [mita2db.hateblo.jp](https://mita2db.hateblo.jp/entry/mysql80-response-time-histgram)
-   2019 [mita2db.hateblo.jp](https://mita2db.hateblo.jp/entry/2019/12/06/233313)

## 参考書籍

[![MySQL徹底入門 第4版 MySQL 8.0対応](https://m.media-amazon.com/images/I/51QQE2jm7CL._SL500_.jpg "MySQL徹底入門 第4版 MySQL 8.0対応")](https://www.amazon.co.jp/exec/obidos/ASIN/B088M1BMBG/iimemo-22/)