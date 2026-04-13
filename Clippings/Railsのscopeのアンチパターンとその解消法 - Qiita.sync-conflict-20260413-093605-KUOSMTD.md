---
Created: Invalid date
URL: https://qiita.com/Seiga/items/e71e9497a395fe61102f
---
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlRTMlODElQUVzY29wZSVFMyU4MSVBRSVFMyU4MiVBMiVFMyU4MyVCMyVFMyU4MyU4MSVFMyU4MyU5MSVFMyU4MiVCRiVFMyU4MyVCQyVFMyU4MyVCMyVFMyU4MSVBOCVFMyU4MSU5RCVFMyU4MSVBRSVFOCVBNyVBMyVFNiVCNiU4OCVFNiVCMyU5NSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MmU3MjgwZWE4YTU5Y2M5OTNkYzAwMzc4YjY5ZWY0Nzk&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwU2VpZ2EmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTA4Zjc1YzQyNmY1YTg4MzFiYzZmZjc3OTQzMDcxYTUy&blend-x=142&blend-y=491&blend-mode=normal&s=97aeaeef5f2057b3ee348ed78a6f71af)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlRTMlODElQUVzY29wZSVFMyU4MSVBRSVFMyU4MiVBMiVFMyU4MyVCMyVFMyU4MyU4MSVFMyU4MyU5MSVFMyU4MiVCRiVFMyU4MyVCQyVFMyU4MyVCMyVFMyU4MSVBOCVFMyU4MSU5RCVFMyU4MSVBRSVFOCVBNyVBMyVFNiVCNiU4OCVFNiVCMyU5NSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MmU3MjgwZWE4YTU5Y2M5OTNkYzAwMzc4YjY5ZWY0Nzk&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwU2VpZ2EmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTA4Zjc1YzQyNmY1YTg4MzFiYzZmZjc3OTQzMDcxYTUy&blend-x=142&blend-y=491&blend-mode=normal&s=97aeaeef5f2057b3ee348ed78a6f71af)

---

本記事は [Classi Advent Calendar 2020](https://qiita.com/advent-calendar/2020/classi) 23日目の記事です。

こんにちは。[@seiga](https://qiita.com/seiga)です。今回はRailsのscopeのアンチパターンとその解消法について解説します。

- *scope(named_scope)**はRails2.1の目玉機能として野良gemからRails本体に取り込まれた経緯を持つ機能です。[RubyKaigi2008で松田さんが話された](https://www.slideshare.net/a_matsuda/new-wave-of-database-programming-with-ruby-19-on-rails-21/)のが周知の初めでしょうか。これまでクエリの組み立てを直接行っていたのが、そのクエリ断片を名前のついたscopeとして定義、動的に組み合わせることで一つの集合とすることが出来るようになりました。

some_model.rb

```
  scope :published, -> { where(published: true) }
```

```
> SomeModel.published == SomeModel.where(published: true)> true
```

ですが便利な分、安易に使うとすぐに質の低いコードを生み出してしまうことになります。

Railsのscopeを使いこなすためには、以下のような点で相応の検討が必要です。

## ユースケースを表現するscopeは地雷。クエリオブジェクトに置き換えろ

scopeは記述されたModelに対し**パブリックなメソッド**として振る舞います。すると以下のような問題が発生します。

二つのControllerで同じscopeが利用されているパターンを考えてみましょう

_Aさんがある日Some1Controllerとそこで呼び出されるscopeを実装しました。scopeがこのユースケース専用の挙動になっているように見えますが、Controllerでクエリ書くよりかはましでしょ、と完了_

app/models/some.rb

```
scope :in_some_scope, ->(param) do  .published  .eager_load(SomeBelongsToModel)  .where(some_belongs_to_model: {someparam: param})end
```

app/controllers/some1_controller.rb

```
SomeModel.in_some_scope(param)
```

_しばらく経った後、BさんはSome2Controllerを実装しました。その際、Modelを眺めたらなんとぴったりのscopeがあるではありませんか！喜んで流用しました_

app/controllers/some2_controller.rb

```
SomeModel.in_some_scope(param)
```

_その後、Some1Controllerでだけ並び順の制御が必要になったため、Aさんはin_some_scopeというscopeにorderを追加します_

app/models/some.rb

```
scope :in_some_scope, ->(param) do  .published  .eager_load(SomeBelongsToModel)  .where(some_belongs_to_model: {someparam: param})  .order(created_at: :desc)end
```

_するとなんということでしょう、Aさんの預かり知らぬところでバグが発生してしましました。それは同じscopeを使っていたBさんの作成したSome2Controllerでした...fin_

このように**scopeは修正に対して弱く、影響範囲も広くなりがち**という特性があります。

上記の例はユースケースを表現していたscopeを安易に使いまわしてしまったことによる**暗黙的な結合**でした。

ユースケースとはざっくり言うと**特定のワンアクション専用の状態**のことです。これをscopeにしてしまった場合、おそらくscopeの命名でしっくりくるものがないはずです。

それに対し、`published`、のように**ドメインとしての抽象化ができているscope**もあります。これは「公開済み」と言うドメインを表現するscopeになるので変更耐性も強く、使いまわしても悪い作用が起こることは少ないです。

some_model.rb

```
  # 特定のアクション状態に依存しない、普遍的な「公開済み」を表すscope  scope :published, -> { where(published: true) }
```

クエリがユースケースによって複雑に組み立てられる場合、scopeをまとめたscopeを新たに用意する、Modelのクラスメソッドにする、などの選択肢があります。が、どちらもパプリックアクセス可能なので別場所で呼び出される可能性があり、潜在的な結合を生む要因となります。

オススメは**クエリオブジェクトを作ってそちらにクエリ組み立てを委譲**する方法です。こうすることで暗黙的な結合を防ぎつつ、ユースケース依存の複雑なクエリを組み立てることができます。

app/controllers/some1_controller.rb

```
::Some::Some1ListQuery.call(param)
```

app/queries/some1/some1_list_query.rb

```
module Some1  class Some1ListQuery    class << self      def call(param)        SomeModel.published                 .eager_load(SomeBelongsToModel)                 .where(some_belongs_to_model: {someparam: param})                 .order(created_at: :desc)      end    end  endend
```

app/controllers/some2_controller.rb

```
::Some::Some2ListQuery.call(param)
```

app/queries/some2/some2_list_query.rb

```
module Some2  class Some2ListQuery    class << self      def call(param)        SomeModel.published                 .eager_load(SomeBelongsToModel)                 .where(some_belongs_to_model: {someparam: param})      end    end  endend
```

こうすることで各ユースケースでの暗黙的な結合が解消され、各自のユースケースのみに集中することができます。ちなみにこの場合クエリオブジェクト内で**ドメインを表現するscope以外にアクセスしないように**しましょう。せっかくユースケースを表現していたクエリオブジェクトの意義が失われます。

条件分岐があるscopeを置き換えたい場合はどうすべきでしょうか？

この場合はthenを使ったチェインを用意することで、同じ構造にすることができます。

app/queries/some/some_list_query.rb

```
      # 公開状態の一覧を取得。タイトル検索、順序指定あれば追加で適応      def call(search_tilte: nil, sort_order: nil)        SomeModel.published                 .then { |relation| search_by_title(relation, search_title) }                 .then { |relation| sort_by_order(relation, sort_order) }      end      private      # 名前検索検索あれば追加      def search_by_title(relation, search_title)        return relation if search_title.blank?        relation.where('title LIKE ?', "%#{search_title}%")      end      # 順序指定で並び替え      def sort_by_order(relation, sort_order)        case sort_order        when 'desc'          relation.order(created_at: :desc)        when 'asc'          relation.order(created_at: :asc)        else          relation        end      end
```

ActiveRecord::Relationは遅延評価なのでこのようにチェインすればscopeのようにクエリが組みあがります。ただしこの際thenの戻り値はActiveRecord::Relationを返すようにしないと、scopeと同じ挙動とは言えないので気をつけましょう。

## scopeのメリットはクエリの抽象化

scopeを使ってドメインを表現する集合を引っ張ってくるときに、どんなカラムにどんな絞り込みをかけるのかは**抽象化**することができます。これはscopeのメリットです。

つまり**どんな手続きでドメインを表現する集合を得るのかを隠蔽**することができます。

が、往往にして

app/models/some.rb

```
scope :by_column_name, ->(param){ where(column_name: param) }
```

のようにscope名に「どのカラムでどんな操作をするか」が記述されたscopeが実装されてしまうことがあります。確かにscopeを挟むことでModelへの直接操作はしていないように見えますが、これだとscope呼び出し側で「どのカラムで絞り込むか」という知識を持っていることになります。つまり

app/controllers/some_controller.rb

```
SomeModel.by_column_name(param)
```

と

app/controllers/some_controller.rb

```
SomeModel.where(column_name: param)
```

は全く同質であり、**隠蔽になっておらずscopeにするのは無意味です**。こうするぐらいならControllerで直接ActiveRecord::Relationメソッドを呼び出しちゃう方がまだマシです。

また、スコープの命名がシンプルすぎるため、他のユースケースでも便利だからとなんとなく利用される場合が発生します。つまり結合度が不用意に増加するため、このスコープの挙動を変えたい時に**結合度に応じて修正コストが肥大化**してしまいます。

scopeの命名では**ドメインを表現する抽象化された集合**の名前をちゃんと考えて命名しましょう。

[最悪弊社VPoTに切腹させられます](https://nekogata.hatenablog.com/entry/2012/12/11/054555)

## scopeの戻り値として設定するのはActiveRecord::Relationだけにすべき

scopeでは戻り値の設定は自由にできます。

some_model.rb

```
  scope :some, -> { 1 }
```

```
> SomeModel.some> 1
```

ただし戻り値がnilやfalseになった場合だけallを返します。

some_model.rb

```
  scope :some, -> { false }
```

```
> SomeModel.some == SomeModel.all> true
```

scopeはscope同士でチェインできる事もメリットです。が、scopeの戻り値がバラバラだった場合、チェインする事なんてできませんよね？scopeのチェイン順に気をつけてチェインしましょう、というのはあまりにも本末転倒です。

また、nilやfalseが置き換えられてallの結果が返るのもコントロール外と感じられますね？

ですので、**scopeの戻り値として設定するのはActiveRecord::Relationだけにすべき**です。

ちなみにscopeがActiveRecord::Relationを返さない実装が可能になっている点については、warningを出そうという提案がなされたことがありますが、パフォーマンスが低下するとのことでクローズ。代わりにドキュメントにて**なるべくActiveRecord::Relationかnilを返そうね**、という形に落ち着いた経緯があります。

[https://github.com/rails/rails/pull/32846](https://github.com/rails/rails/pull/32846)

[https://github.com/rails/rails/issues/34532](https://github.com/rails/rails/issues/34532)

また、Rails5.2までのガイドでは

> どのスコープメソッドも、常にActiveRecord::Relationオブジェクトを返します

となっており、いかにもフレームワーク側で戻り値をActiveRecord::Relationに変換しているような書き方になっていて紛らわしかったのですが、

Rails6のガイドでは

> どのスコープボディも、ActiveRecord::Relationかnilを返す必要があります

という文言に[変更されています](https://github.com/rails/rails/pull/34534)。

## scopeはクラスメソッドのエイリアスではない

Railsガイドには

> スコープでのメソッドの設定は、クラスメソッドの定義と完全に同じ (というよりクラスメソッドの定義そのもの)です。

という記述があったのですが[削除されました。](https://github.com/gmcgibbon/rails/commit/b91dc579286415699a842c3d0a136dba541815b8)

経緯としては、[このstackoverflowの回答](https://stackoverflow.com/questions/32930312/ruby-on-rails-activerecord-scopes-vs-class-methods)を軸に

`スコープが常にActiveRecord::Relationオブジェクトを返すという点を除いては、クラスメソッドの定義と*ほぼ*同じ`と言う文言に改変する[PRがマージされた](https://github.com/rails/rails/pull/33653)のですが、

改変された説明が、既にあった[条件文の使用](https://github.com/gmcgibbon/rails/blob/8b4471a9b427f5816b4911c6879c582701546e66/guides/source/active_record_querying.md#using-conditionals)の内容と重複することになったため削除されたという流れです

> ただし１つ注意点があります。それは条件文を評価した結果がfalseになった場合であっても、スコープは常にActiveRecord::Relationオブジェクトを返すという点です。クラスメソッドの場合はnilを返すので、この振る舞いが異なります。したがって、条件文を使ってクラスメソッドをチェインさせていて、かつ、条件文のいずれかがfalseを返す場合、NoMethodErrorを発生することがあります。

よって、Rails6.1のRails Guideにおいては、引数付きのscopeはクラスメソッドに置き換えた方が良いとの記述は[消されています](https://github.com/gmcgibbon/rails/commit/3dbe9a50d764679ce15a3ccb005edd3d5a494114)。上記のように**scopeとクラスメソッドの挙動は異なる**ためです。

## default scope is evil

これはもう一大ムーブメントのようなものがあって叩かれまくっているわけで、知らない方はこのセクションタイトルで調べていただくとして、弊社ではこれ起因のバグ踏んだ経験多く反対派が多数です。unscopeやイニシャライズ時に注意して使えば便利だと思うのですが、地雷度高いので厳しいですね...ちなみに私はnested_attributesとacts_as_paranoidとcomposite_primary_keyの合わせ技を食らって**大変な目**にあったことがありますので反対派です。

## まとめ

「Railsのscopeを使いこなす」なんて事を言いながら、半分ぐらい脱scopeの話をしていたような...?

scopeは確かに便利です。一見簡単にクエリ隠蔽できるのでかるーくいっぱい使いたくなる気持ちはとてもよく分かります。ですがその分アンチパタンな使い方されやすいと感じます。何事も用法用量、発行されるクエリが果たしていいものなのか、潜在的なリスク生んでないかなど、scopeにすべき部分そうでない部分、しっかり考えて使っていく必要がありますね。

## 参考文献

[https://github.com/rails/rails/blob/master/guides/source/active_record_querying.md](https://github.com/rails/rails/blob/master/guides/source/active_record_querying.md)

[https://nekogata.hatenablog.com/entry/2012/12/11/054555](https://nekogata.hatenablog.com/entry/2012/12/11/054555)

[https://qiita.com/SoarTec-lab/items/6e5f7781edf8d3fe4889](https://qiita.com/SoarTec-lab/items/6e5f7781edf8d3fe4889)

[https://www.slideshare.net/moro/namedscope-more-detail-webcareer-presentation](https://www.slideshare.net/moro/namedscope-more-detail-webcareer-presentation)

[https://techracho.bpsinc.jp/hachi8833/2018_06_14/57526](https://techracho.bpsinc.jp/hachi8833/2018_06_14/57526)

[https://techracho.bpsinc.jp/hachi8833/2018_11_13/64284](https://techracho.bpsinc.jp/hachi8833/2018_11_13/64284)

Register as a new user and use Qiita more conveniently

1. You get articles that match your needs
2. You can efficiently read back useful information

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FSeiga%2Fitems%2Fe71e9497a395fe61102f&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FSeiga%2Fitems%2Fe71e9497a395fe61102f&realm=qiita)

Comments