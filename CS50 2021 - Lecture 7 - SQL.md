---
Tags:
  - CS50
---
## SQLとは

データテーブルの操作はcsvやspreadsheetでできるが、数百万行のデータセットを開くには膨大なメモリが必要になる。そのような時にはSQLが最も軽量かつ高速なツールで適している。SQLはデータベースを操作するための言語。

  

## インデックス

SQLで検索すると高速に処理してくれるが、関係するカラムにインデックスを張ることでその処理速度はより速くなる。これはそのカラムにBTree構造を作るため。検索速度が向上する一方でインデックス作成にメモリを使用するため、性能とメモリ使用量とのトレードオフになる

## SQLインジェクション

フォームにSQL文を含めることでシステム内部で実行されるSQLを書き換えて、機密情報を入手するハッキング手法。

例えば、メールアドレスのフォームに`hoge@example.com’—`と入力する。システム内部でSQLインジェクション対策をしてない場合は次のようなコードになっている

```
SQL.execute("SELECT * FROM users WHERE email = #{email} AND password = #{password}")
```

このSQLでは`—`はコメントアウトとみなされるので↓のように解釈される

```
SELECT * FROM users WHERE email = 'hoge@example.com'--' AND password = #{password}
```

結果、`hoge@example.com`の個人情報が抜き取られる。他にもSQLインジェクションはさまざまな方法がある。ちゃんとプレースホルダーを使って対策しよう。

```
SQL.execute("SELECT * FROM users WHERE email = ? AND password = ?", email, password)
```

## 競合 race conditions

同じレコードを複数ユーザーで同時にUPDATEしようとすると競合が起きる。このような時はロックする。ロックはデータの一貫性を担保するが、ユーザーを待たせることにもなるのでトレードオフになる。