---
URL: https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738
---
> 更新情報: 2013/11/19: 初版公開 2021/01/08: 訳文見直し、追記

こんにちは、hachi8833です。今回は、自分が知りたかった、Active Recordモデルのリファクタリングに関する記事を翻訳いたしました。1年前の記事なのでRails 3が前提ですが、Rails 4以降でも基本的には変わらないと思います。リンクは可能なものについては日本語のものに置き換えています。

なお、ここでご紹介したオブジェクトは、app以下にそれぞれ以下のようにフォルダを追加してそこに配置します。

> 注記: 以下は使われそうなフォルダを列挙しただけであり、実際にはこの一部しか使いません。

1. [Value Object](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#value-object)
2. [Service Object](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#service-object)
3. [Form Object](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#form-object)
4. [View Object](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#view-object)
5. [Policy Object](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#policy-object)
6. [Decorator](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#decorator)

## [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#translate) 肥大化したActive Recordモデルをリファクタリングする7つの方法（翻訳）

Railsアプリケーションの品質を高めるためにチーム内で[Code Climate](https://codeclimate.com/)を使用していれば、モデルの肥大化を自然と避けるようになるでしょう。モデルが肥大化(ファットモデル)すると、大規模アプリケーションのメンテナンスが困難になります。ファットモデルは、コントローラがドメインロジックで散らかってしまうよりは1段階だけましであるとはいえ、たいていの場合[単一責任の原則 (Single Responsibility Principle: SRP)](https://code.tutsplus.com/ja/tutorials/solid-part-1-the-single-responsibility-principle--net-36074)の適用に失敗した状態であると言えます。

SRPの適用は、元々難しいものではありません。ActiveRecordクラスは永続性と関連付けを扱うものであり、それ以外のものではありません。しかしクラスはじわじわ成長していきます。永続性について本質的に責任を持つオブジェクトは、やがて事実上ビジネスロジックも持つようになるのです。1年2年が経過すると、`User` クラスには500行ものコードがはびこり、パブリックなインターフェイスには数百ものメソッドが追加されることでしょう。それに続くのはコールバック地獄です。

アプリケーションに何か本質的に複雑な要素を追加したら、ちょうどケーキのタネをケーキ型に流し込むのと同じように、それらを小規模かつカプセル化されたオブジェクト群(あるいはより上位のモジュール)に整然と配置することが目標になります。ファットモデルは、さしずめタネをケーキ型に流し込むときに見つかるダマ(混ざらなかった粉の固まり)のようなものでしょう。これらのダマを砕いて、ロジックが等分に広がって配置されるようにしなければなりません。これを繰り返し、それらが最終的にシンプルできちんと定義されたインターフェイスを持つ一連のオブジェクトとなって、それらが見事に協調動作するようにしましょう。

そうは言っても、きっとこう思う人もいることでしょう。

> “でもRailsでちゃんとOOPするのってめちゃくちゃ大変ぢゃなくね?!”

私も以前は同じことを思ってました。でも若干の調査と実践の結果、RailsというフレームワークはOOPを妨げてなどいないという結論に達しました。スケールできないでいるのはRailsのフレームワークではなく、従来のRailsの慣習・流儀の方です。より具体的に言えば、Active Recordパターンできちんと扱える範囲を超えるような複雑な要素を扱うための定番の手法がまだないのです。幸いにも、オブジェクト指向における一般的な原則とベストプラクティスというものがあるので、Railsに欠けている部分にこれらを適用することができます。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#1) （その前に）ファットモデルからミックスインで展開しないこと

肥大化したActiveRecordクラスから単に一連のメソッドを切り出して[concerns](https://api.rubyonrails.org/v6.1.0/classes/ActiveSupport/Concern.html)やモジュールに移動するのはよくありません。移動したところで、後でまた1つのモデルの中でミックスインされてしまうのですから。いつだったか、こんなことを言っていた人がいました。

> “app/concerns ディレクトリを使っているようなアプリケーションって、だいたい後から頭痛の種になる(=concerning)んだよね”

私もそう思います。ミックスインよりも、継承で構成する方がよいと思います継承よりコンポジションの方がよいと思います。このようなミックスインは、部屋に散らかっているガラクタを引き出しに押し込めてピシャリと閉めたのと変わりません。一見片付いているように見えても、引き出しの中はぐちゃぐちゃ、どこに何があるのかを調べるだけでも大変です。ドメインモデルを明らかにするために必要な分解と再構成を実装するのも並大抵ではありません。  
これはもうリファクタリングするしかないでしょう。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#value-object) 1. Value Objectに切り出す

Value Objectは、異なるオブジェクト同士であっても値が等しければ等しいと見なされる、シンプルなオブジェクトです。Value Objectは変更不可能であるのが普通です。Rubyの標準ライブラリには`Date`、`URI`、`Pathname` などのValue がありますが、Railsアプリケーションでもドメイン固有のValue Objectを定義できますし、そうすべきです。ActiveRecordからValue Objectへの展開は、すぐにもメリットの得られるリファクタリングです。

Railsでは、ロジックが関連付けられている属性が1つ以上ある場合にはValue Objectが有用です。単なるテキストフィールドやカウンタ以上の要素は、何でもValue Objectの候補になりえます。

ちょうど著者が仕事をしている某テキストメッセージングアプリケーションには、`PhoneNumber` というValue Objectがあります。そして某Eコマースアプリケーションでは`Money`クラスを必要としています。私たちのCode Climateには `Rating`というValue Objectがあり、受け取ったクラスやモジュールのランキングをAからFまでの段階で表します。ここではRuby の`String`クラスのインスタンスを使うこともできます(実際使っていました)が、この`Rating`を使用すると以下のように振る舞いとデータを一体化することができます。

```
class Rating  include Comparable  def self.from_cost(cost)    if cost <= 2      new("A")    elsif cost <= 4      new("B")    elsif cost <= 8      new("C")    elsif cost <= 16      new("D")    else      new("F")    end  end  def initialize(letter)    @letter = letter  end  def better_than?(other)    self > other  end  def <=>(other)    other.to_s <=> to_s  end  def hash    @letter.hash  end  def eql?(other)    to_s == other.to_s  end  def to_s    @letter.to_s  endend
```

次にすべての`ConstantSnapshot`で`Rating`のインスタンスをパブリックなインターフェイスに公開します。

```
class ConstantSnapshot < ActiveRecord::Base  # …  def rating    @rating ||= Rating.from_cost(cost)  endend
```

このパターンは、`ConstantSnapshot`がスリムになるだけでなく、他にも多くの利点があります。

- `\#worse_than?`メソッドと`\#better_than?`メソッドは、レートを比較する場合には`<`や`>`などのRubyの組み込み演算子よりも適切です。
- `\#hash`や`\#eql?`を定義しておけば`Rating`をハッシュキーとして使用できます。Code Climateではこれを用いて、定数をレートごとに`Enumberable\#group_by`でグループ化しています。
- `\#to_s`メソッドを定義してあるので、`Rating`を簡単に文字列やテンプレートに変換できます。
- このクラス定義は、ファクトリーメソッドを導入する場合にも便利です。矯正コスト (=クラスの「[臭い](http://d.hatena.ne.jp/tbpg/20120428/1335626089)」を除去するのにかかる時間) に見合う正しい `Rating` を得られます。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#service-object) 2. Service Objectに切り出す

アクションによってはService Objectを用いて操作をカプセル化できることもあります。著者の場合、以下の基準に1つ以上マッチすればService Objectの導入を検討します。

- アクションが複雑になる場合 (決算期の終わりに帳簿をクローズする、など)
- アクションが複数のモデルにわたって動作する場合 (eコマースの購入で`Order`, `CreditCard`, `Customer` を使用する、など)
- アクションから外部サービスとやりとりする場合 (SNSに投稿する、など)
- アクションが背後のモデルの中核をなすものではない場合 (一定期間ごとに古くなったデータを消去する、など)
- アクションの実行方法が多岐にわたる場合 (認証をアクセストークンやパスワードで行なう、など)。これはGoF (Gang of Four)の書籍で言う[Strategyパターン](http://ja.wikipedia.org/wiki/Strategy_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)です。

例として、`User\#authenticate`メソッドを取り出して `UserAuthenticator`に配置しましょう。

```
class UserAuthenticator  def initialize(user)    @user = user  end  def authenticate(unencrypted_password)    return false unless @user    if BCrypt::Password.new(@user.password_digest) == unencrypted_password      @user    else      false    end  endend
```

このとき、`SessionsController` は以下のような感じになります。

```
class SessionsController < ApplicationController  def create    user = User.where(email: params[:email]).first    if UserAuthenticator.new(user).authenticate(params[:password])      self.current_user = user      redirect_to dashboard_path    else      flash[:alert] = "Login failed."      render "new"    end  endend
```

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#form-object) 3. Form Objectに切り出す

1つのフォーム送信で複数のActive Recordモデルを更新する場合、Form Objectを使用して集約することができます。Form Objectを使えば、(個人的には使用を避けたい) [`accepts_nested_attributes_for`](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html)よりもずっときれいなコードになります。`Company` と `User`を同時に作成するユーザー登録フォームを例にとってみましょう。

```
class Signup  include Virtus  extend ActiveModel::Naming  include ActiveModel::Conversion  include ActiveModel::Validations  attr_reader :user  attr_reader :company  attribute :name, String  attribute :company_name, String  attribute :email, String  validates :email, presence: true  # …その他のバリデーション …  # フォームそのものは決して永続化しない  def persisted?    false  end  def save    if valid?      persist!      true    else      false    end  endprivate  def persist!    @company = Company.create!(name: company_name)    @user = @company.users.create!(name: name, email: email)  endend
```

これらのオブジェクトでは[Virtus](https://github.com/solnic/virtus) gemのActive Record的な属性機能を利用しています。Form ObjectはActive Recordと同様に振る舞うので、コントローラは通常と変わらないものになります。

```
class SignupsController < ApplicationController  def create    @signup = Signup.new(params[:signup])    if @signup.save      redirect_to dashboard_path    else      render "new"    end  endend
```

Form Objectは、上のようなシンプルな例ではうまくいきますが、永続性のロジックが含まれていてフォームが複雑になるのであれば、Service Objectも併用するのがよいでしょう。

Form Objectを導入することでさらにボーナスが付きます。バリデーションのロジックはコンテキストに依存しがちですが、Active Record自身の中でバリデーションを走らせるという融通の効かない方法に代えて、バリデーションロジックを実際に必要な場所で定義できます。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#note) 訳追記（2021/01/08）

Rails 5からはActive Recordで[attributes API](https://techracho.bpsinc.jp/hachi8833/2017_12_11/48422)も使えるようになりました。記事末尾の関連記事もどうぞ。

Rails 5.2からはActive Modelでもattributes APIを使えるようになりました。

参考: [【Rails】「ActiveModel::Attributes」が便利という話 - 日々の学びのアウトプットするブログ](https://ushinji.hatenablog.com/entry/2019/06/17/220611)

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#query-object) 4. Query Objectに切り出す

スコープやクラスメソッドなどのActiveRecordサブクラスが乱雑に定義された、複雑なSQLクエリがある場合は、Query Objectに切り出すことを検討します。1つのQuery Objectは、ビジネスロジックに基づいた結果セットを1つだけ返す責務を担当します。

たとえば、誰も訪問していないお試しを検索するQuery Objectは以下のような感じになります。

```
class AbandonedTrialQuery  def initialize(relation = Account.scoped)    @relation = relation  end  def find_each(&block)    @relation.      where(plan: nil, invites_count: 0).      find_each(&block)  endend
```

このオブジェクトを使ってバックグラウンドで以下のようにメールを送信します。

```
AbandonedTrialQuery.new.find_each do |account|  account.send_offer_for_supportend
```

ActiveRecord::RelationインスタンスはRails 3によって[ファーストクラスオブジェクト](http://ja.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E7%B4%9A%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)として扱われるため、Query Objectでも多くの機能を使えます。このおかげで、[コンポジション](http://qiita.com/mikamikuh@github/items/1cdcd8b25a2e23f10525)を使ってクエリを結合できます。

```
old_accounts = Account.where("created_at < ?", 1.month.ago)old_abandoned_trials = AbandonedTrialQuery.new(old_accounts)
```

この種のクラスは個別にテストする必要はありません。オブジェクトのテストとデータベースのテストは同時に行うようにし、それによって正しい行が正しい順序で返されることと、join（結合）や[eager loading](http://d.hatena.ne.jp/willnet/20090303/1236093728)がすべて動作する（[N + 1クエリ問題](http://www.techscore.com/blog/2012/12/25/rails%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E7%B4%B9%E4%BB%8B-n1%E5%95%8F%E9%A1%8C%E3%82%92%E6%A4%9C%E5%87%BA%E3%81%99%E3%82%8B%E3%80%8Cbullet%E3%80%8D/)などを回避できているなど）ことを確認します。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#view-object) 5. View Objectを導入する

表示にしか使わないようなロジックが必要な場合、それはモデルに置くべきではありません。「仮にアプリケーションのUIががらりと変わったら(たとえば音声駆動UIになったとしたら)、その時にもこれをモデルに置く必要があるだろうか」と自問自答してみましょう。モデルに置く必要のない表示ロジックであることがわかったら、ヘルパーに置くか、できればなるべくView Objectに置くようにしましょう。

たとえば、[Code Climate](https://codeclimate.com/github/rails/rails)ではRails on Code Climateなどでコードベースのスナップショットに基づいたクラスのレート付けを行う円グラフを使用していますが、これらは次のようにView Objectにカプセル化できます。

```
class DonutChart  def initialize(snapshot)    @snapshot = snapshot  end  def cache_key    @snapshot.id.to_s  end  def data    # @snapshotからデータを取り出してJSON構造に変換するコードを置く  endend
```

ところで、私はビューとERBテンプレート（またはHamlやSlim）が一対一対応していることが多いのに気が付きました。そこで、Railsで使える[Two Step Viewパターン](http://martinfowler.com/eaaCatalog/twoStepView.html)を実装できないか調べ始めたのですが、今のところこれについては明快なソリューションを見つけられずにいます。

Railsコミュニティでよく使われている「Presenter」という用語についてですが、この用語が他の用法と重複したり誤解を招いたりする可能性があるため、著者はこの用語を避けるようにしています。Presenterという語は、本記事で言うところのForm Objectを説明するために[Jay Fieldsによって導入されました](http://blog.jayfields.com/2007/03/rails-presenter-pattern.html)。また、運の悪いことにRailsでは「View」という用語もいわゆる「(ビューの)テンプレート」を指すものとして使われています。曖昧さを避けるため、著者はView Objectを「Viewモデル」と書くことがあります。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#policy-object) 6. Policy Objectに切り出す

複雑な読み出し操作はそのオブジェクト自身で行なうのがふさわしいことがあります。私はこのような場合にPolicy Objectを検討します。Policy Objectを使うことで、本質的でないロジック (分析用にどのユーザーをアクティブとみなすか、など) を、中核となるドメインオブジェクトから切り離すことができます。以下は例です。

```
class ActiveUserPolicy  def initialize(user)    @user = user  end  def active?    @user.email_confirmed? &&    @user.last_login_at > 14.days.ago  endend
```

このPolicy Objectには1つのビジネスルールがカプセル化されています。このビジネスルールでは、emailが確認済みで、かつ2週間以内にログインしたことがあるユーザーをアクティブなユーザーとみなすようになっています。Policy Objectは、複数のビジネスルール (特定のデータへのアクセスを許可する`Authorizer` など) をカプセル化することもできます。

Policy ObjectはService Objectと似ていますが、私は「Service Objectは書き込み操作用」「Policy Objectは読み出し操作用」と使い分けています。これらはQuery Objectとも似ていますが、Policy Objectはメモリに読み込み済みのドメインモデルについて操作を行なうのに対し、Query Objectは特定の結果セットを返す「SQLの実行」に特化している点が異なります。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#decorator) 7. Decoratorに切り出す

Decoratorは既存の操作に関する機能を階層化することによって、コールバックとよく似た機能を果たします。Decoratorは、特定の環境でしか実行したくないコールバックロジックがある場合や、ある機能をモデルに含めるとモデルの責務が増え過ぎる（=モデルが肥大化する）場合に便利です。

あるブログ投稿にコメントが付くと誰かのFacebookウォールに自動的に投稿されるようにしたとします。この場合、このロジック自体を`Comment`クラスにハードコードしなければならないわけではありません。コールバックに負わせる責務が多すぎると、テストの実行が遅くなり、不安定になるという形で兆候が現れます。こうした副作用は、何の関連もないテストケースから取り除くべきでしょう。

Facebookへの投稿ロジックをDecoracorに展開する方法を以下に示します。

```
class FacebookCommentNotifier  def initialize(comment)    @comment = comment  end  def save    @comment.save && post_to_wall  endprivate  def post_to_wall    Facebook.post(title: @comment.title, user: @comment.author)  endend
```

このDecoratorをコントローラで以下のように使います。

```
class CommentsController < ApplicationController  def create    @comment = FacebookCommentNotifier.new(Comment.new(params[:comment]))    if @comment.save      redirect_to blog_path, notice: "Your comment was posted."    else      render "new"    end  endend
```

DecoratorはService Objectとは異なります。Service Objectは既存のインターフェイスに対する責務を階層化しますが、これをDecorator化すると、`FacebookCommentNotifier` インスタンスをあたかも単なる`Comment`であるかのように取り扱います。

Rubyは、メタプログラミングを使用して[Decoratorを簡単に作成するための仕組み](http://robots.thoughtbot.com/evaluating-alternative-decorator-implementations-in)を標準ライブラリに多数備えています。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738#wrapup) 最後に

複雑なモデル層をうまく取り扱うためのツールは、Railsアプリケーションにも多数存在します。これらのツールを使用するためにRailsを捨てる必要などありません。Active Recordsは素晴らしいライブラリですが、これだけに頼っていてはどんなパターンも失敗します。ActiveRecordsは、極力永続的な振る舞いにとどめておくようにしてください。本記事で紹介したテクニックを一部だけでも適用して、自分のアプリケーションのドメインモデルに固まっているロジックを分散させることができれば、アプリケーションのメンテナンスはずっと容易になるでしょう。

本記事で紹介したパターンの多くはシンプルです。これらのオブジェクトは、いずれも「昔ながらのPORO（Plain Old Ruby Object: シンプルなRubyオブジェクト）」であって、ただその使い方が異なるだけです。これはOOPの一部であり、OOPの美しさをなすものです。問題を解決するのにフレームワークやライブラリだけに頼る必要はありません。手法に適切な名前を付けることも重要な技法です。

本記事で紹介した7つの手法はいかがでしたでしょうか。お気に入りの手法は見つかりましたでしょうか。それはどんな理由でしょうか。皆さまのコメントをお待ちしています。

追伸: この記事を気に入っていただけましたら、[元記事](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)の下にあるフォームからCode Climateのニュースレターをぜひ購読してみてください。OOPやRailsアプリケーションのリファクタリングなど、今回の記事のようなトピックを扱っています。記事のボリュームは控えめにしています。

### より詳しい情報

本記事をレビューしてくれた皆様に感謝します: _Steven Bristol, Piotr Solnica, Don Morrison, Jason Roelofs, Giles Bowkett, Justin Ko, Ernie Miller, Steve Klabnik, Pat Maddox, Sergey Nartimov, Nick Gauthier_

## 関連記事