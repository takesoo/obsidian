---
Tags:
  - Ruby_on_Rails
Status: Done
Last edited time: Invalid date
Created time: Invalid date
---
## preload

N+1になるテーブルを先にselectする

```
class Blog < ApplicationRecord  has_many :entries endclass Entry < ApplicationRecord  belongs_to :blogend# 子テーブル(?)側の先読みBlog.preload(:entries).where(id: [1, 2])# SELECT * FROM blogs WHERE id IN (1, 2)# SELECT * FROM entries WHERE blog_id IN (1, 2)# 子テーブルでの絞り込みはできないBlog.preload(:entries).where(entrires: { id: [1, 2] } )# PG::UndefinedTable - ERROR:  missing FROM-clause entry for table "entrires"# LINE 1: SELECT "blogs".* FROM "blogs" WHERE "entrires"."id" IN ($1, ...#
```

## eager_load

LEFT_OUTER_JOIN

```
Entry.eager_load(:comments).where(id: [*3..5])# SELECT *#   FROM entries E LEFT OUTER JOIN comments C ON C.entry_id = E.id#  WHERE E.id IN (3, 4, 5)# 子テーブルでの絞り込みが可能Blog.eager_load(entries: :comments).where(entries: {id: [1, 2]})# SELECT * #   FROM blogs B #        LEFT OUTER JOIN entries E ON E.blog_id = B.id #        LEFT OUTER JOIN comments C ON C.entry_id = E.id # WHERE E.id IN (1, 2)
```

## includes

SQLにwhere句があるかないかでpreloadとeager_loadを自動的に切り替える。

予期せぬ挙動をしたりするので使わないほうがいいらしい

## joins

INNER_JOIN

```
Blog.joins(:entries)    .where(id: 1)    .select("blogs.*, entries.title AS entry_title, entries.body")# SELECT blogs.*, entries.title AS entry_title, entries.body#   FROM blogs#        INNER JOIN entries#        ON entries.blog_id = blogs.id#  WHERE blogs.id = 1
```

## merge

AR::Releation同士の結合

```
class User < ApplicationRecord  has_many :books endclass Book < ApplicationRecord  belongs_to :userend# nameがbooks.nameになってしまうuser.books.where(name: "igaiga").to_sql#=> "SELECT "books".* FROM "books" WHERE "books"."user_id" = 1 AND "books"."name" = 'igaiga'"# mergeを使えばusers.nameと指定できるuser.books.merge(User.where(name: "igaiga")).to_sql#=> "SELECT "books".* FROM "books" WHERE "books"."user_id" = 1 AND "users"."name" = 'igaiga'"
```

  

## 引用

[

ActiveRecordのjoinsとpreloadとincludesとeager_loadの違い - Qiita

ActiveRecordでN+1クエリを潰すためにeager loadingを行う場合、 preloadや includesや eager_loadが役に立つ。 Preload, Eagerload, Includes and Joins という記事にそれらの違いがよくまとめられているんだけど、includesが挙動を変える条件があまり正確に書かれていなくて自信が持てなかったし、そもそも記事が古いのでRails4.1.5のソースを読んで調べた。 せっかく調べたので、全体を通して日本語でまとめてみようと思う。 デフォルトでINNER JOINを行う。LEFT OUTER JOINを行いたい時は left_joinsを使う。 他の3つとの違いは、associationをキャッシュしないこと。 associationをキャッシュしないのでeager loadingには使えないが、ActiveRecordのオブジェクトをキャッシュしない分メモリの消費を抑えられる。 なので、JOINして条件を絞り込みたいが、JOINするテーブルのデータを使わない場合は joinsを使うのが良い。 User.joins(*).where(posts: {*})とか User.joins(*).merge(Post.*) みたいに使う。 指定したassociationをLEFT OUTER JOINで引いてキャッシュする。 クエリの数が1個で済むので場合によっては preloadより速い。 JOINしているので、 preloadと違って、 joins と同じようにJOINしたテーブルで絞込ができる。 指定したassociationを複数のクエリに分けて引いてキャッシュする。 複数のassociationをeager loadingするときとか、あまりJOINしたくないでかいテーブルを扱うときはpreloadを使うのがよさそう。 preloadしたテーブルによって絞り込もうとすると、例外を投げる。 includesしたテーブルでwhereによる絞り込みを行っている includesしたassociationに対してjoinsかreferencesも呼んでいる 任意のassociationに対してeager_loadも呼んでいる のうちいずれかを満たす場合、 eager_loadと同じ挙動(LEFT JOIN)を行い、 そうでなければ preloadと同じ挙動(クエリを分けて実行)をする。 絞り込みが必要な時に例外を投げず eager_loadにfallbackする preload 。 根拠 ActiveRecord::Relation\#eager_loading?が trueの場合LEFT JOINを行い、 falseの場合 クエリを分けて読み込む。 eager_load_valuesとか includes_valuesはeager_loadしたやつとincludesしたときに追加されるやつで、そのまま。 joined_includes_values はincludesしたやつをわざわざjoinsもしてる奴のリスト。 references_eager_loaded_tables?はJOIN元以外に referencesが行われている場合にtrueになる。なお、whereで別テーブルによる絞り込みを行った場合も referencesが行われる ため、この場合もtrueになる。 ちなみに、referencesはincludesのスイッチング専用メソッドである。 そのテーブルとのJOINを禁止したいケースでは preloadを指定し、JOINしても問題なくてとりあえずeager loadingしたい場合は includesを使い、必ずJOINしたい場合は eager_load を使いましょう。

![](https://cdn.qiita.com/assets/favicons/public/production-c620d3e403342b1022967ba5e3db1aaa.ico)https://qiita.com/k0kubun/items/80c5a5494f53bb88dc58

![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9QWN0aXZlUmVjb3JkJUUzJTgxJUFFam9pbnMlRTMlODElQThwcmVsb2FkJUUzJTgxJUE4aW5jbHVkZXMlRTMlODElQThlYWdlcl9sb2FkJUUzJTgxJUFFJUU5JTgxJTk1JUUzJTgxJTg0JnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LWNsaXA9ZWxsaXBzaXMmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz0zYWUxMzhhMWQ5YjkyMjQwM2EwZmU4YTY2Zjk2ZmQ1OQ&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwazBrdWJ1biZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9NzMwOWJiNmM5ZGM2ZDlkMTIwN2E1NTZkM2E5ZTZmZTU&blend-x=142&blend-y=491&blend-mode=normal&s=b33f210e77b6021f4c7a44a569b1b83a)](https://qiita.com/k0kubun/items/80c5a5494f53bb88dc58)

[

ActiveRecord ～ 複数テーブルにまたがる検索（preload, eager_load, include, joins） - Qiita

preload, eager_load, include, joins が実際にどういうSQLを吐くかを確かめた。※Rails4/5両方で確認。 ※説明に使用している各テーブルは blogs ← entries ← comments...

![](https://cdn.qiita.com/assets/favicons/public/apple-touch-icon-ec5ba42a24ae923f16825592efdc356f.png)https://qiita.com/leon-joel/items/f26556c9e56833983856

![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9QWN0aXZlUmVjb3JkJTIwJUVGJUJEJTlFJTIwJUU4JUE0JTg3JUU2JTk1JUIwJUUzJTgzJTg2JUUzJTgzJUJDJUUzJTgzJTk2JUUzJTgzJUFCJUUzJTgxJUFCJUUzJTgxJUJFJUUzJTgxJTlGJUUzJTgxJThDJUUzJTgyJThCJUU2JUE0JTlDJUU3JUI0JUEyJUVGJUJDJTg4cHJlbG9hZCUyQyUyMGVhZ2VyX2xvYWQlMkMlMjBpbmNsdWRlJTJDJTIwam9pbnMlRUYlQkMlODkmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT01NiZ0eHQtY2xpcD1lbGxpcHNpcyZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPWQ3MDkyZWJkODQxZmJkMjEyMTBiZDNjYzhjYWFlYTBi&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwbGVvbi1qb2VsJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz1mYmQzODg4NDdiY2VlYzNjODlhMGRlMmZhNGIxNDdkMA&blend-x=142&blend-y=491&blend-mode=normal&s=2a2a5ad8c6d7be935e5e975aa8b43af4)](https://qiita.com/leon-joel/items/f26556c9e56833983856)

[

[ActiveRecord] mergeメソッド｜Railsの練習帳

![](https://zenn.dev/images/icon.png)https://zenn.dev/igaiga/books/rails-practice-note/viewer/ar_merge

![](https://res.cloudinary.com/zenn/image/upload/s--Vp83YK2h--/g_center%2Ch_280%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYm9va19jb3Zlci84ZGNkODAxYmZjLnBuZw==%2Cw_200/v1627283836/default/og-base-book_yz4z02.jpg)](https://zenn.dev/igaiga/books/rails-practice-note/viewer/ar_merge)