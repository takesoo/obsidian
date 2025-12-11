---
URL: https://techracho.bpsinc.jp/hachi8833/2023_02_14/53537
---
[![](https://techracho.bpsinc.jp/wp-content/uploads/2018/03/rails_say_no_to_chained_scopes_eyecatch-min.png)](https://techracho.bpsinc.jp/wp-content/uploads/2018/03/rails_say_no_to_chained_scopes_eyecatch-min.png)

---

原著者の許諾を得て翻訳・公開いたします。

- 英語記事: [Say no to chained scopes!](http://craftingruby.com/posts/2015/06/24/say-no-to_chained-scopes.html)
- 原文公開日: 2015/06/24
- 著者: [Jeroen Weeink](http://craftingruby.com/)
- サイト: [Crafting Ruby](http://craftingruby.com/)

日本語タイトルは内容に即したものにしました。

- 2018/04/18: 初版公開
- 2023/02/14: 更新

## Rails: スコープをモデルの外でチェインするのはやめておけ（翻訳）

Railsアプリで、次のようにモデルのデータベーススキーマの内部にまで立ち入っている（コントローラ）コードをよく見かけます。

```
class Person < ActiveRecord::Base  enum gender: { male: 1, female: 2 }end
```

```
class PeopleController < ApplicationController  def index    @people = Person.where(gender: Person.genders[:male])                    .where('age >= 18')                    .where(right_handed: false)    respond_to(:html)  endend
```

このコードにはいくつか問題点があります。

- コントローラがモデルのデータベース構造に関して持っている知識が多すぎます。背後の詳細な情報が上位の層に漏れると、背後の構造を変更しにくくなります。
- メソッド呼び出しをチェインしているので、モックを使ったテストが死ぬほどやりづらくなります。

このような実装の詳細はモデル内にカプセル化しなければなりません。ActiveRecordのスコープの助けを借りて何とかしてみましょう。

```
class Person < ActiveRecord::Base  enum gender: { male: 1, female: 2 }  scope :male,        -> { where(gender: 1) }  scope :adult,       -> { where('age >= 18') }  scope :left_handed, -> { where(right_handed: false) }end
```

```
class PeopleController < ApplicationController  def index    @people = Person.male.adult.left_handed    respond_to(:html)  endend
```

生SQLやモデル属性の知識はモデル内にカプセル化されました。これで一件落着…したのでしょうか？

テストの書きやすさはほんの少しだけましになりましたが、異なるスコープを組み合わせる長いメソッドチェインはまだ残っています。コントローラをテストするには、これまでと同様にモック軍団を出動させなければなりません。

```
class PeopleControllerTest < ActionController::TestCase  def test_people_index    adult_finder        = mock    left_handed_finder  = mock    Person.expects(:male).returns(adult_finder)    adult_finder.expects(:adult).returns(left_handed_finder)    left_handed_finder.expects(:left_handed)    get :index    assert_response :success  endend
```

テストコードは`expects`が多く、しかもかなり脆くなっています。たとえテスト対象コードが正常に動作していても、スコープの順序がどこかで変わればテストは失敗してしまいます。

スコープが複雑になると他にも問題が生じることがあります。スコープはいくらでも自由に組み合わせられますが、その組み合わせから正しいSQLが生成されるとは限りません。その組み合わせを全部テストしていたら心が削られてしまいます。

私は、スコープをモデルの外でチェインするのではなく、スコープの組み合わせをモデル内で単一のスコープやクラスメソッドにまとめるのが好みです。これなら処理を可能な限り内部化できますし、データベースクエリの最適化などの作業もずっとやりやすくなります。

```
class Person < ActiveRecord::Base  enum gender: { male: 1, female: 2 }  scope :male,        -> { where(gender: 1) }  scope :adult,       -> { where('age >= 18') }  scope :left_handed, -> { where(right_handed: false) }  class << self    def left_handed_male_adults      left_handed.male.adult    end  endend
```

```
class PeopleController < ApplicationController  def index    @people = Person.left_handed_male_adults    respond_to(:html)  endend
```

スコープチェインは`Person.left_handed_male_adults`クラスメソッドの内部にラップされています。必要ならこのクラスメソッド自身をスコープとして定義することも可能な点にご注目ください。2つの方法の大きな違いは、スコープがActiveRecordリレーションを返すことを保証するかどうかです。

スコープの組み合わせはぐっとシンプルになり、しかもテストに対して頑丈になります。

```
class PeopleControllerTest < ActionController::TestCase  def test_people_index    Person.expects(:left_handed_male_adults)    get :index    assert_response :success  endend
```

関連するモデルの外でスコープをチェインするのを避ければ、コードベースの癒着も減り、それによってメンテナンスやリファクタリングもやりやすくなります。

もちろんあらゆるスコープはpublicなので、このスコープもその気になればチェインできます。話を複雑にしないために、スコープをモデルの外でチェインしたくなる衝動をこらえてください。

## 関連

[Railsのdefault_scopeは使うな、絶対（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_11_04/47302)

概要 原著者の許諾を得て翻訳・公開いたします。 英語記事: Don’t use default_scope. Ever. 公開日: 2017/10/01 著者: Andy Croll — フリーランスのRuby開発者です。 2017/10/31: 初版公開 2021/11/04: 更新 Railsのdefault_scopeは使うな、絶対（翻訳） あるモデル全体にスコープを適用したい場合、default_scopeが利用できます。詳しくはRailsガイド: Active Recordクエリインターフェイス 14.スコープ（日本語）かRails APIドキュメントをご覧ください。 投稿を非表示にできる機 … [続きを読む Railsのdefault_scopeは使うな、絶対（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_11_04/47302)

[Rails tips: モデルのクエリをカプセル化する2つの方法（翻訳）](https://techracho.bpsinc.jp/hachi8833/2018_01_16/51052)

概要 原著者の許諾を得て翻訳・公開いたします。 英語記事: Encapsulating queries in a Rails model – Ruby on Rails / ActiveRecord 原文公開日: 2018/01/01 著者: Paweł Dąbrowski Rails: モデルのクエリをカプセル化する2つの方法（翻訳） 要点: サービスやコントローラなどのクラスからデータベースクエリのロジックを分離することは間違いなく優れた方法です。ロジックをモデルに置く場合、次の2とおりの方法が使えます。 1. クラスメソッド化する def self.recent order(created_at … [続きを読む Rails tips: モデルのクエリをカプセル化する2つの方法（翻訳）](https://techracho.bpsinc.jp/hachi8833/2018_01_16/51052)