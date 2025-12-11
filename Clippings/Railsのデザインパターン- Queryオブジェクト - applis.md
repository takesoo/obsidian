---
URL: https://applis.io/posts/rails-design-pattern-query-objects
---
RailsのデザインパターンのひとつにQueryオブジェクトがあります。これはコントローラからActiveRecordモデルに対する絞り込みなどの操作を、ひとつの責務としてクラスに切り出すパターンです。コントローラの肥大化を防ぎ、またテストが書きやすくなります。このQueryオブジェクトについて示します。

Railsのおすすめな本

Ruby on Railsアプリケーションの開発や学習に役立つ本を、「[Ruby on Railsのおすすめな本](https://applis.io/posts/ruby-on-rails-books)」で紹介しています。あわせてご覧ください。

Hiroki Zenigami

テクニカルライター。元エンジニア。共著で「現場で使えるRuby on Rails 5」を書きました。プログラミング教室を作るのが目標です。

[Follow @zenizh](https://twitter.com/zenizh)

スポンサーリンク

## [Queryオブジェクトとは](https://applis.io/posts/rails-design-pattern-query-objects#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%A8%E3%81%AF)

以下、ActiveRecord::Relationを単にRelationと表記します。Queryオブジェクトは「Relationに対し結合や絞り込み、ソートなどの操作を定義し、Relationを返すクラス」です。ActiveRecordモデルのscopeとして設定することで、チェーンの一部として使用できるようになります。

よく見られるQueryオブジェクトの例として、Relationではなく配列などほかのクラスを返すものがあります。ただ、クラスやデザインパターンからはインタフェースや返り値が予想できるべきです。「QueryオブジェクトはRelationを返す」というルールのもとに設計することで、コードの品質が保たれることにつながります。

## [Queryオブジェクトの必要性](https://applis.io/posts/rails-design-pattern-query-objects#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%BF%85%E8%A6%81%E6%80%A7)

Queryオブジェクトは「コントローラからクエリ操作の責務を分離し、ActiveRecordモデルと疎結合に保つため」に必要なデザインパターンです。

コントローラからクエリを操作する場合、複雑なクエリは長くなり、肥大化してしまいます。また正しいRelationが取得できているかのテストが書きづらくなります。再利用性もありません。

たとえば「1日以内に記事を投稿したユーザーの一覧」を取得するコードを見てみます。コントローラに書くと、次のようになります。

```
class UsersController < ApplicationController  def index    @users = User.joins(:posts)      .where(        posts: {          published_at: 1.day.ago..        }      )      .order(created_at: :desc)  endend
```

この例ならまだいいですが、これに「PVが100以上の記事を書いたユーザー」「フォロワーが3人以上ついたユーザー」のように条件がふえていくと、コントローラがどんどん肥大化していきます。

コントローラの責務はモデル層に命令を出し、ビュー層にデータを渡すことにあります。モデル層の操作を組み立てることではありません。コントローラがモデル層の知識を知りすぎると、モデル層の変更の影響を受けてしまいます。

Queryオブジェクトは、コントローラからクエリ操作の責務を分離し、クラス間を疎結合に保つためのデザインパターンです。

スポンサーリンク

## [Queryオブジェクトの例](https://applis.io/posts/rails-design-pattern-query-objects#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E4%BE%8B)

ここでは、上述したユーザー一覧の例をQueryオブジェクトで書いてみます。

### [動作環境](https://applis.io/posts/rails-design-pattern-query-objects#%E5%8B%95%E4%BD%9C%E7%92%B0%E5%A2%83)

この記事にあるコードは次の各バージョンで動作を確認しています。

|   |   |
|---|---|
|名前|バージョン|
|Ruby|2.7.1|
|Ruby on Rails|6.0.3.2|

### [コントローラ](https://applis.io/posts/rails-design-pattern-query-objects#%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%A9)

まず、Queryオブジェクトの使われ方を把握するために、コントローラを見てみます。コントローラからは次のようにActiveRecordモデルのscopeとして呼び出します。引数として日付を指定しています。

```
# app/controllers/users_controller.rbclass UsersController < ApplicationController  def index    @users = User.recently_posted(3.days)  endend
```

### [ActiveRecordモデル](https://applis.io/posts/rails-design-pattern-query-objects#activerecord%E3%83%A2%E3%83%87%E3%83%AB)

次にActiveRecordモデルです。scopeとしてQueryオブジェクトを渡しています。AcitveRecordモデルのscopeにすることで、「返ってくるのがそのモデルのRelationである」ことが自明になるというメリットがあります。

```
# app/models/user.rbclass User < ApplicationRecord  scope :recently_posted, RecentlyPostedUsersQueryend
```

### [Queryオブジェクト](https://applis.io/posts/rails-design-pattern-query-objects#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

最後にQueryオブジェクトです。QueryオブジェクトのベースとなるQueryクラスと、それを継承したRecentlyPostedUsersQueryクラスを定義しています。

ActiveRecordモデルのscopeとしてオブジェクトを渡すと、そのオブジェクトの`\#call`メソッドが呼ばれます。これを`\#new`に委譲することで、Queryオブジェクトの`#call`が実行される仕組みです。`#call`に引数を定義することで、コントローラから渡せるようになります。

```
# app/queries/query.rbclass Query  class << self    delegate :call, to: :new  end  def call    raise NotImplementedError  end  private  attr_reader :relationend# app/queries/recently_posted_users_query.rbclass RecentlyPostedUsersQuery < Query  DEFAULT_FROM = 1.day  def initialize(relation = User.all)    @relation = relation  end  def call(from = DEFAULT_FROM)    relation      .joins(:posts)      .where(        posts: {          updated_at: from.ago..        }      )  endend
```

以上となります。コントローラからRelationに対する操作の責務をひとつのクラスに分離できました。ActiveRecordモデルに変更があってもコントローラへの影響がなくなり、またテストも書きやすくなりました。

## [Queryオブジェクトのルール](https://applis.io/posts/rails-design-pattern-query-objects#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E3%83%AB%E3%83%BC%E3%83%AB)

以上の内容をもとに、Queryオブジェクトを設計するときのルールについてまとめます。

- ファイルは`app/queries`に配置する
- ベースとなるQueryクラスを定義する
- 接尾辞にQueryをつける
- クラス名から返るRelationが予想できる名前にする
- `\#call`を定義し、ActiveRecord::Relationクラスのオブジェクトを返す
- ActiveRecordモデルのscopeをとおしてのみ使用する。これにより、返るオブジェクトのクラスが自明となる
- `\#call`は副作用のないメソッドにする

### [アンチパターン](https://applis.io/posts/rails-design-pattern-query-objects#%E3%82%A2%E3%83%B3%E3%83%81%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)

Queryオブジェクトの例として、ひとつのクラスに複数のpublicメソッドを定義する例を見かけます。これはひとつのクラスに複数の責務をもつことになります。ある責務の変更が別の責務のメソッドに影響を及ぼすため、避けるべきだと考えます。

ひとつの責務をひとつのクラスに切り出し、単一責任の原則を守って設計することで、保守しやすいコードにすることができます。

### [あわせて読みたい](https://applis.io/posts/rails-design-pattern-query-objects#%E3%81%82%E3%82%8F%E3%81%9B%E3%81%A6%E8%AA%AD%E3%81%BF%E3%81%9F%E3%81%84)