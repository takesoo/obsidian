---
URL: https://qiita.com/furaji/items/12cef3ec4d092865af88
---
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlMjAtJTIwQWN0aXZlUmVjb3JkJTIwJUUzJTgxJUFFJTIwc2NvcGUlMjAlRTMlODIlOTIlMjBRdWVyeSUyMG9iamVjdCUyMCVFMyU4MSVBNyVFNSVBRSU5RiVFOCVBMyU4NSVFMyU4MSU5OSVFMyU4MiU4QiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9ZjY1MTFjMDcyNGFhZTlhZGNlNDU1MzY3ZjM5MTBhOGI&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwZnVyYWppJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04OTBlZWExNGMxMGEzM2RhMmIzZTZkYzFmMDkzNmNlYg&blend-x=142&blend-y=491&blend-mode=normal&s=a23758d4833c68c59744df856304b8ec)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlMjAtJTIwQWN0aXZlUmVjb3JkJTIwJUUzJTgxJUFFJTIwc2NvcGUlMjAlRTMlODIlOTIlMjBRdWVyeSUyMG9iamVjdCUyMCVFMyU4MSVBNyVFNSVBRSU5RiVFOCVBMyU4NSVFMyU4MSU5OSVFMyU4MiU4QiZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9ZjY1MTFjMDcyNGFhZTlhZGNlNDU1MzY3ZjM5MTBhOGI&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwZnVyYWppJnR4dC1jb2xvcj0lMjMyMTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LWFsaWduPWxlZnQlMkN0b3Amcz04OTBlZWExNGMxMGEzM2RhMmIzZTZkYzFmMDkzNmNlYg&blend-x=142&blend-y=491&blend-mode=normal&s=a23758d4833c68c59744df856304b8ec)

---

[Ruby on Rails Advent Calendar 2015](https://qiita.com/advent-calendar/2015/rails) Day 16

updated at 2020-05-09

[Rails4](https://qiita.com/tags/rails4)

この記事は Ruby on Rails Advent Calendar 2015 16日目の記事です(投稿が無かったので過去に書いた記事をねじ込みました)

**※2020/05/09追記 Finder Object についての記事を書きました。**

[https://furaji.hatenablog.jp/entry/2020/05/09/043924](https://furaji.hatenablog.jp/entry/2020/05/09/043924)

## 参考記事

[http://d.hatena.ne.jp/asakichy/20120821/1345505916](http://d.hatena.ne.jp/asakichy/20120821/1345505916)

[http://craftingruby.com/posts/2015/06/29/query-objects-through-scopes.html](http://craftingruby.com/posts/2015/06/29/query-objects-through-scopes.html) ※本投稿はこの記事を翻訳してまとめたものです。

## Query object とは

それ自身が、SQLクエリーとして機能するオブジェクト構造

ActiveRecord にある複雑なSQLクエリを抽出するのに便利

# 実装

## ActiveRecord からの抽出

以下の ActiveRecord のモデルは人気おすすめ動画を返す`scope`を持っています。

複雑なSQLクエリではないですが、この例を用いて Query object についてまとめていきます。

```
class Video < ActiveRecord::Base  scope :featured_and_popular,        -> { where(featured: true).where('views_count > ?', 100) }end
```

`scope`は簡単に、Query object に抽出することができます。

```
module Videos  class FeaturedAndPopularQuery    def initialize(relation = Video.all)      @relation = relation    end    def featured_and_popular      @relation.where(featured: true).where('views_count > ?', 100)    end  endend
```

モデルの`scope`を呼ぶ代わりに、Query object を呼ぶことができます。

```
Videos::FeaturedAndPopularQuery.new.featured_and_popular
```

`Video`クラスからロジックを抽出できたのは良いですが、このままでは既存の`Video.featured_and_popular`呼び出し箇所に影響が出てしまいます。

Query object にロジックを抽出し、かつ呼び出しのインターフェイスが変わることが無ければもっとスマートです。

これを実現する方法を見つけるために、まずは、`scope`メソッドの実装を見てみましょう。

## scopeメソッドの確認

```
def scope(name, body, &block)  unless body.respond_to?(:call)    raise ArgumentError, 'The scope body needs to be callable.'  end  # ...  singleton_class.send(:define_method, name) do |*args|    scope = all.scoping { body.call(*args) }    # ...    scope || all  endend
```

1. `body`は`call`を呼び出せれば任意のオブジェクトで良い
2. `scoping`メソッドはレシーバの`scope`+`body.call(*args)`の`scope`を結果として利用する

`scoping`メソッドは現在のスコープが`body.call(*args)`で絞りこまれたものが返されます。

この例はちょっと複雑なので下記のドキュメントを読むとわかりやすいかもしれません。

[http://apidock.com/rails/ActiveRecord/Relation/scoping](http://apidock.com/rails/ActiveRecord/Relation/scoping)

## scope で Query object を利用

単にスコープから Query object を呼び出すことができます。

`scoping`のおかげで、Query object に現在のスコープを渡す必要はありません。

```
class Video < ActiveRecord::Base  scope :featured_and_popular,        -> { Videos::FeaturedAndPopularQuery.new.featured_and_popular }end
```

しかし、これは非常に冗長になります。

`scope`の実装で見てきたように、`body`は`call`を呼び出せるオブジェクトであることを想定しています。

今、`proc`を渡していますが、`call`を呼び出せる Query object に置き換えることができます。

メソッド名を変更して Query object におきかえてみましょう。

```
module Videos  class FeaturedAndPopularQuery    def initialize(relation = Video.all)      @relation = relation    end    def call      @relation.where(featured: true).where('views_count > ?', 100)    end  endend
```

`proc`は、スコープ宣言から削除され、Query object 自体がスコープの`body`になることができます。

```
class Video < ActiveRecord::Base  scope :featured_and_popular, Videos::FeaturedAndPopularQuery.newend
```

実はこれをさらに短くすることができます。

`Videos::FeaturedAndPopularQuery`は、`call`というクラスメソッドを持つことができ、

`new`に`call`を委譲させることが可能です。

```
module Videos  class FeaturedAndPopularQuery    class << self      delegate :call, to: :new    end    # ↑は↓と同意味です    # def self.call    #   new.call    # end    def initialize(relation = Video.all)      @relation = relation    end    def call      @relation.where(featured: true).where('views_count > ?', 100)    end  endend
```

最終的にはこのように明瞭な記述ができます。

```
class Video < ActiveRecord::Base  scope :featured_and_popular, Videos::FeaturedAndPopularQueryend
```

クエリのロジックは、Query object に抽出されましたが、コードの他の部分を変更する必要がないように`scope`はそのまま残されています。

`scope`は、基本的に Query object に委任されながら、 ActiveRecord のモデルは、きれいに見えるようになりました。

### 補足

ところどころ翻訳が雑なので何かあれば編集リクエストを投げてもらえるとありがたいです。

# 元記事に書かれていないこと

## ディレクトリ構造

```
▾ app/  ▸ assets/  ▸ controllers/  ▸ forms/  ▸ helpers/  ▸ mailers/  ▾ models/      video.rb  ▾ queries/    ▾ videos/        featured_and_popular_query.rb      query.rb  ▸ services/  ▸ views/
```

私はこのように、`queries`ディレクトリ以下に Query object を設置しました。

`scope`に定義する前提で、以下のようにQueryクラスを継承する設計にしています。

- query.rb

```
class Query  class << self    delegate :call, to: :new  endend
```

- videos/featured_and_popular_query.rb

```
module Videos  class FeaturedAndPopularQuery < Query    def initialize(relation = Video.all)      @relation = relation    end    def call      @relation.where(featured: true).where('views_count > ?', 100)    end  endend
```

## おまけ

### 引数を用いた例

- videos/featured_and_popular_query.rb

```
module Videos  class FeaturedAndPopularQuery < Query    def initialize(relation = Video.all)      @relation = relation    end    def call(views_count)      @relation.where(featured: true).where('views_count > ?', views_count)    end  endend
```

と書き換え、

`Video.featured_and_popular(1000)`

のように呼び出すことができます。

### arelを用いた例

`where('views_count > ?', views_count)`

ってなんかダサいですよね？

そんな時は`arel`が使えます。

- videos/featured_and_popular_query.rb

```
module Videos  class FeaturedAndPopularQuery < Query    def initialize(relation = Video.all)      @relation = relation    end    def arel_views_count      Video.arel_table[:views_count]    end    def call(views_count)      @relation.where(featured: true).where(arel_views_count.gt(views_count))    end  endend
```

## まとめ

Query object を用いた事により ActiveRecord のモデルがすっきりしました。

今回は簡単な例を取り上げましたが、複雑なクエリになるほど力を発揮してくると思います。

Register as a new user and use Qiita more conveniently

1. You get articles that match your needs
2. You can efficiently read back useful information

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Ffuraji%2Fitems%2F12cef3ec4d092865af88&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Ffuraji%2Fitems%2F12cef3ec4d092865af88&realm=qiita)

Comments