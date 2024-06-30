---
finished reading: false
---
# Railsのパターンとアンチパターン1: 概要編（翻訳）｜TechRacho by BPS株式会社
  #ReadItLater 
 #ReadableArticle

## articleURL
https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446

## siteName
TechRacho

## date
2024-06-30

## articleContent
### 概要

原著者の許諾を得て翻訳・公開いたします。

-   英語記事: [Introduction to Ruby on Rails Patterns and Anti-patterns | AppSignal Blog](https://blog.appsignal.com/2020/08/05/introduction-to-ruby-on-rails-patterns-and-anti-patterns.html)
-   原文公開日: 2020/08/05
-   著者: [Nikola Đuza](https://pragmaticpineapple.com/): ハンガリーNovi Sad在住のエンジニア兼ライター、ブログや登壇で知識の普及に努めています。JavaScriptやRubyで面白いものを作るのが好きです。
-   サイト: [AppSignal Blog](https://blog.appsignal.com/)

日本語タイトルは内容に即したものにしました。

「[Railsのパターンとアンチパターンシリーズ](https://techracho.bpsinc.jp/tag/rails-pattern-antipattern)」その1へようこそ。このシリーズでは、Railsアプリで仕事をしているときに出会うであろうあらゆる種類のパターンについて深く調べていきます。

今回は、パターン（デザインパターン）の概要を紹介するとともに、アンチパターンの概要についても解説します。よりわかりやすく説明するために、かなり前から世の中で使われているRuby on Railsフレームワークを用います。Railsが何らかの理由でお気に召さないとしても、本記事でご紹介するアイデアやパターンは、皆さんの手に馴染んでいる他の技術と共鳴することがあるかもしれません。

パターンとアンチパターンについて詳しく説明する前に、皆さんにお尋ねします。「パターンが必要になった経緯はどのようなものですか？」「あらゆるパターンが自分たちのソフトウェアで必要となる理由は何ですか？」「ソリューションを設計する必要がある理由は何ですか？」

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#1) あなたは（単なるプログラマーではなく）設計者です

コンピュータプログラミングの黎明期においても、プログラムを書くときに設計を避けて通ることはできませんでした。プログラム（ソフトウェア）を書くということは、すなわち問題解決の方法（ソリューション）を設計することです。プログラムを書いているときのあなたは設計者です（実際の肩書はどうぞご自由に）。私たちが書くソフトウェアは自分たちだけのものではなく、他の人も読んだり編集したりするものでもあるので、ソリューションを正しく設計することは重要です。

エンジニアは何世代にもわたって、これらのパターンをすべて頭に置いたうえで、コードやアーキテクチャからこれまで業務で目にした類似の設計を見出してきました。そして設計を抽出し、標準的な問題解決手法をドキュメント化してきました。こうした作業は、人間の性質に沿った自然な手法であると考える人もいます。人間は、あらゆるものごとを[分類](https://en.wikipedia.org/wiki/Principles_of_grouping)して[パターンを見い出す](https://ja.wikipedia.org/wiki/%E3%82%B2%E3%82%B7%E3%83%A5%E3%82%BF%E3%83%AB%E3%83%88%E5%BF%83%E7%90%86%E5%AD%A6#%E3%83%97%E3%83%AC%E3%82%B0%E3%83%8A%E3%83%B3%E3%83%84%E3%81%AE%E6%B3%95%E5%89%87)ことを好みます。ソフトウェアについても例外ではありません。

ソフトウェアが複雑になるにつれて、私たちは人間としてさまざまなパターンを見い出すようになってきました。そして世界中のエンジニアたちが力を合わせてソフトウェアのデザインパターンを発展させ、一定の形にまとめるようになりました。デザインパターンに関する書籍やエッセイや講演もたくさんあり、十分練り上げられ現場での実績を積んだソリューションに関するアイデアがさらに広く知られるようになってきました。そしてこうしたソリューションによって、工数や費用が大きく削減されました。そこで、デザインパターンという用語を改めて考察し、その真の意味を考えてみることにしましょう。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#2) デザインパターンとは何か

ソフトウェアエンジニアリングにおける「パターン」とは、よくある問題を解決するときに再利用できるソリューションであると説明されます。ソフトウェアエンジニアは、こうしたパターンを何らかの形で有用なプラクティスとみなしています。ソフトウェアエンジニアはパターンで考えるので、あっという間にパターンと真逆のアンチパターンに陥ることもあります（アンチパターンについては後述します）。

あるデザインパターンは問題解決の道筋を示してくれますが、ソフトウェアのその他の部分とうまく調和する具体的なコード片を教えてくれるわけではありません。パターンは、十分に設計されたコードを書くためのガイドであると考えられますが、実装を思い付かなければ何にもなりません。日々のコーディングでパターンが用いられるようになった1980年代後半には、Kent BeckとWardn Cunninghamが「[パターン言語](http://c2.com/doc/oopsla87.html)」を使うことを思いつきました。

パターン言語というアイデアそのものは、1970年代後半にChristopher Alexanderの『[A Pattern Language](https://www.goodreads.com/book/show/79766.A_Pattern_Language)』に示されています。しかし意外にも、同書はソフトウェアエンジニアリングについてではなく、建築におけるアーキテクチャについて書かれていました。パターン言語とは、一貫した方法で編成されたさまざまなパターンのセットであり、1つのパターンは1つの問題を記述するとともに、さまざまに応用可能な問題解決方法の核心部分を記述します。どこかで見覚えがありませんか？（ヒント1: フレームワーク）（ヒント2: Rails）

やがて、1994年に[GoF（Gang Of Four）](http://wiki.c2.com/?GangOfFour)が著した伝説の書籍『[Design Patterns](https://www.goodreads.com/book/show/85009.Design_Patterns)』（日本語版『[オブジェクト指向における再利用のためのデザインパターン](https://www.amazon.co.jp//dp/4797311126/)』）が刊行されると、デザインパターンはソフトウェアエンジニアリング界隈で広く知れ渡りました。同書では、現在も用いられている有名な「Factoryパターン」「Singletonパターン」「Decoratorパターン」を含む多くのパターンについて解説と定義を行っています。

デザインとパターンのおさらいはこのぐらいにして、次はアンチパターンとは何かを考えてみましょう。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#3) 設計のアンチパターンとは何か

パターンが善玉だとすれば、アンチパターンは悪玉ということになります。もう少し正確に言うと、ソフトウェアのアンチパターンは「よく使われているが、非効率または生産性を下げると考えられるパターン」のことです。アンチパターンの典型的な例が「Godオブジェクト」で、他のオブジェクトに切り出すか分離できるはずの機能や依存関係が全部盛りされている強欲なオブジェクトです。

アンチパターンに共通する原因はひとつではなく、いろいろな原因があります。「善玉パターンが悪玉（アンチパターン）に豹変する」はその好例です。たとえば、あなたが以前在籍していた企業で特定の技術をよく用いていて、その技術については高度なレベルに達していたとします。説明用に、たとえばそれがDockerだとしましょう。あなたは、アプリケーションをDockerコンテナに効率よくパッキングしてクラウド上でオーケストレーションし、クラウドからログを引っ張ってくる技術を身に付けています。そしてある日突然、あなたがフロントエンドアプリケーションをリリースする必要のある新しい仕事を得たとしましょう。あなたはDockerおよびDockerでアプリをリリースする手法について知り尽くしているので、パッケージに一切合財を押し込んでクラウドにデプロイする方法を最初に決定しました。

しかし、現職のフロントエンドアプリはDockerが必要なほど複雑ではないことをほぼ見落としてしまいました。このアプリをコンテナに押し込めるソリューションは必ずしも効率が最大とは限りません。一見よさそうなアイデアでしたが、ゆくゆくは生産性が落ちてしまうことが明らかになります。これは「[Golden Hammer](https://en.wikipedia.org/wiki/Law_of_the_instrument)（打ち出の小槌）」と呼ばれるアンチパターンです。

「打ち出の小槌」アンチパターンとは要するに「ハンマーを持つ者には、あらゆるものが釘に見える」ということです。Dockerやサービスのオーケストレーションに精通している人には、あらゆるものがクラウド上でオーケストレーションされるように作られたDockerサービスに見えてしまうのです。

このアンチパターンはこれまでも発生していましたし、今後も発生するでしょう。善玉が悪玉に豹変することもあれば、その逆もあります。RubyやRailsではどんなときに当てはまるのでしょうか？

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#4) まずRubyを学べ、Railsはその次

多くの人が、Ruby on Railsを使うようになって初めてRubyを使い始めました。Ruby on Railsは、Webサイトを短期間で構築できる有名なWebフレームワークです。私もご多分に漏れずRailsで初めてRubyに触れましたが、それ自体は何の問題もありません。Railsは「MVC（Model-View-Controller）」と呼ばれる十分確立したソフトウェアパターンをベースとしています。ただし、本記事でRailsのMVCパターンについて深く調べる前に、ひとつ大きな勘違いについて触れておきます。「Rubyを正しく学ばずにRailsに手を出してしまう」というものです。

かつてのRailsフレームワークは、アプリケーションの構想を得てそれを短期間で構築したいときにはとても頼りになるフレームワークのひとつでした。もちろんRailsは現在も使われていますが、全盛期ほどではありません。Railsは利用も実行も簡単なので、多くの初心者が`rails new`コマンドで自分のWebアプリを作り始めました。その後何が起こったかというと、もろもろの問題が噴出し始めたのです。初心者はRailsの開発スピードやシンプルさ、そして何もかもが魔法のようにスムーズに進められることに魅了されます。やがて「魔法」を当たり前と思うようになり、舞台裏でどんなことが起きているのかを理解できないままになります。

私も同じ問題に遭遇しましたし、多くの初心者や初級者が今も同じ問題に苦しめられていると思います。フレームワークを手に入れてアプリを構築するまではよいのですが、フレームワークの魔法にばかり頼っていたために、何か凝ったことをしようとした途端に挫折してしまいます。ここで大事なのは、勇気を出して初心に帰り、基礎を学び直すことです。後戻りするのは思ったほど大変ではありませんし、それがベストな方法です。しかしRubyなどの基本を学ばないまま進み続けると、問題は目に見えて大きくなります。『[The Well-Grounded Rubyist](https://www.goodreads.com/book/show/3892688-the-well-grounded-rubyist)』は、こんなときの助けになるおすすめの良書のひとつです。

あなたが初心者なら、同書を最初から最後までみっちり読みとおす必要はありません。その代わり、本をすぐ横に置いていつでも参照できるようにしておきましょう。「今すぐ自分がやっていることをすべて中止して本を全部読め」と申し上げたいのではありません。そうではなく、ときには作業の手を止めて、Rubyの基礎知識をリフレッシュする時間を確保しましょうと申し上げたいのです。きっと新しい地平線が見えてくることでしょう。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5) MVC: Railsになくてはならないもの

なるほど、ではMVCはどうでしょうか？MVCパターンは長年使われ続けており、Ruby（Rails）やPython（Django）、Java（PlayやSpring MVC）など、さまざまな言語の無数のフレームワークで採用されています。MVCの考え方は、以下のように独立したコンポーネントで作業を行うというものです。

モデル（Model）

データやビジネスロジックを扱う

ビュー（View）

データの表現方法やユーザーインターフェイス

コントローラ（Controller）

「モデルのデータを取得する」「ビューをユーザーに表示する」の2つを結びつける

理論上はよくできていそうですし、ロジックが最小限かつWebサイトに複雑なロジックがない場合は非常にうまくいきます。そこから先はややこしいことになりますが、これについては後述します。

MVCパターンは、燎原の火のようにWeb開発コミュニティ全体に急速に広まりました。近年異常なほど人気を集めているReactのようなライブラリですら、Webアプリのビューレイヤとして説明できます。これほど不動の人気を勝ち得たパターンはMVC以外にはありません。RailsはAction Cableで[Publish-Subscribe](https://ja.wikipedia.org/wiki/%E5%87%BA%E7%89%88-%E8%B3%BC%E8%AA%AD%E5%9E%8B%E3%83%A2%E3%83%87%E3%83%AB)（Pub/Sub）パターンを追加しましたが、[チャネルの概念](https://railsguides.jp/action_cable_overview.html#%E7%94%A8%E8%AA%9E%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)はMVCパターンのコントローラとして説明されています。

さて、これほどまで広く用いられているMVCにもアンチパターンはあるのでしょうか？そこで、MVCにある個別のコンポーネントで最もよく目にするアンチパターンをいくつか見てみることにしましょう。

#### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-1) モデルの問題

アプリケーションが成長してビジネスロジックが拡張されると、モデルに多くのものが押し込まれる傾向があります。このまま成長し続けると「ファットモデル（fat model）」というアンチパターンになる可能性があります。

Railsの「モデルは厚くせよ、コントローラは薄くせよ」という有名なパターンは、悪玉とみなされることもあれば善玉とみなされることもあります。私たちは、ファットなものを抱え込むことは何であれアンチパターンだと考えています。この点をよりよく理解するために、ひとつ例をご紹介します。SpotifyやDeezerのようなストリーミングサービスを運営するとしましょう。このサービスの内部には、以下のように曲（歌）を扱うSongモデルがひとつあります。

```
class Song < ApplicationRecord
  belongs_to :album
  belongs_to :artist
  belongs_to :publisher

  has_one :text
  has_many :downloads

  validates :artist_id, presence: true
  validates :publisher_id, presence: true

  after_update :alert_artist_followers
  after_update :alert_publisher

  def alert_artist_followers
    return if unreleased?

    artist.followers.each { |follower| follower.notify(self) }
  end

  def alert_publisher
    PublisherMailer.song_email(publisher, self).deliver_now
  end

  def includes_profanities?
    text.scan_for_profanities.any?
  end

  def user_downloaded?(user)
    user.library.has_song?(self)
  end

  def find_published_from_artist_with_albums
    ...
  end

  def find_published_with_albums
    ...
  end

  def to_wav
    ...
  end

  def to_mp3
    ...
  end

  def to_flac
    ...
  end
end
```

このようなモデルの問題は、曲（歌）に関係しそうなさまざまなロジックが雑然と置かれたゴミ捨て場のようになってしまっていることです。モデルにメソッドがひとつずつ追加され続けると、長年経つうちにこのようにモデル全体が巨大かつ複雑になります。ロジックを切り出してさまざまな場所に移動しておけば、将来プロジェクトにとって役に立つ可能性があります。

このモデルを反面教師とすることで、今すぐ皆さんにおすすめできる設計上のよい習慣をいくつも見いだせます。まず、このモデルは[単一責任の原則](https://ja.wikipedia.org/wiki/SOLID)（SRP: Single Responsibility Principle）に違反しています。このモデルはフォロワーや音楽出版社への通知を扱い、テキストが公序良俗に反するかどうかをチェックし、さまざまなオーディオフォーマットの曲をエクスポートするメソッドがいくつもあり...という具合に責務を抱え込みすぎています。そのせいでモデルが複雑になってしまい、モデルのテストをどう書けばいいのか私には見当もつかないほどです。

このモデルをリファクタリングする方法は、「メソッドがどのように呼び出されるか」「他の場所でどのように使われるか」によって大きく変わります。ここではそうしたケースを扱うための一般的なアイデアをいくつかご紹介しますので、その中から皆さんのケースに最もよく合うものをお使いいただけます。

##### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-1-1) コールバックをジョブに切り出す

フォロワーや音楽出版社への通知を行うコールバックは、ジョブに切り出せます。以下のようにこのジョブをキューに入れることで、ロジックをモデルの外に追い出せます。

```
class NotifyFollowers < ApplicationJob
  def perform(followers)
    followers.each { |follower| follower.notify }
  end
end

class NotifyPublisher < ApplicationJob
  def perform(publisher, song)
    PublisherMailer.song_email(publisher, self).deliver_now
  end
end
```

ジョブはそれ用の別プロセスで実行され、モデルから切り離されます。これでジョブのロジックを別のテストで検証できるようになり、ジョブがモデルから正しくキューに送られるかどうかをチェックすれば済むようになります。

##### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-1-2) Decoratorパターンを使う

公序良俗に反する書き込みのチェックと、ユーザーが曲をダウンロードしたかどうかのチェックは、いずれもアプリのビューで発生します。このような場合は[Decoratorパターン](https://ja.wikipedia.org/wiki/Decorator_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)が利用できます。[Draper](https://github.com/drapergem/draper) gemはDecoratorを手軽に利用できるソリューションとして人気があります。このgemを使って、以下のようなDecoratorを書けます。

```
class SongDecorator < Draper::Decorator
  delegate_all

  def includes_profanities?
    object.text.scan_for_profanities.any?
  end

  def user_downloaded?(user)
    object.user.library.has_song?(self)
  end
end
```

続いて、コントローラで以下のように`decorate`を呼びます。

```
def show
  @song = Song.find(params[:id]).decorate
end
```

呼び出した結果をビューで以下のように利用します。

```
<%= @song.includes_profanities? %>
<%= @song.user_downloaded?(user) %>
```

gemを追加するのが好みでなければ、自分でDecoratorをこしらえる方法もありです（これについては別記事にする予定です）。モデルの「関心（concern）」の大部分を切り離すことに成功したので、今度は曲を検索するメソッドと曲を変換するメソッドに手を付けましょう。これらは、以下のように専用のモジュールを作ることで分離できます。

```
module SongFinders
  def find_published_from_artist_with_albums
    ...
  end

  def find_published_with_albums
    ...
  end
end

module SongConverter
  def to_wav
    ...
  end

  def to_mp3
    ...
  end

  def to_flac
    ...
  end
end
```

Songモデルで`SongFinders`モジュールを`extend`すると、モジュールのメソッドをクラスメソッドとして利用できるようになります。Songモデルで`SongConverter`モジュールを`include`すると、モジュールのメソッドをモデルのインスタンスメソッドとして利用できるようになります。

以上のリファクタリングをすべて行ったことで、Songモデルがかなりすっきりしました。

```
class Song < ApplicationRecord
  extend SongFinders
  include SongConverter

  belongs_to :album
  belongs_to :artist
  belongs_to :publisher

  has_one :text
  has_many :downloads

  validates :artist_id, presence: true
  validates :publisher_id, presence: true

  after_update :alert_artist_followers, if: :published?
  after_update :alert_publisher

  def alert_artist_followers
    NotifyFollowers.perform_later(self)
  end

  def alert_publisher
    NotifyPublisher.perform_later(publisher, self)
  end
end
```

モデルのアンチパターンはこの他にもいろいろあります。ここでご紹介したのは、モデルでどんな間違いが起きる可能性があるかを示すほんの一例にすぎません。モデルに関する他のアンチパターンについては、本シリーズの続編で扱いますのでご期待ください。とりあえず今はここまでとし、続いてビューでどんな間違いが起きるかを見ていきましょう。

#### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-2) ビューの問題

Rails開発者は、複雑なモデルに加えて、複雑なビューにも悩まされることがあります。その昔、Webアプリケーションのビュー王国を支配していたのはHTMLとCSSでした。やがて時とともにJavaScriptがじわじわとビュー王国に支配の手を伸ばし、とうとうフロントエンドのほとんどすべてがJavaScriptで書かれるようになりました。Railsフレームワークは、これに関して少し異なるパラダイムに従っています。ビューを全面的にJavaScriptに委ねるのではなく、JavaScriptコード片をビューに「振りかける」だけです。

いずれにしろ、HTMLとCSSとJavaScriptとRubyを一箇所でまとめて扱うのは面倒になりがちです。Railsでビューを構築するときにややこしいのは、ドメインロジックが（モデルではなく）ビューの中に入ってしまう場合がある点です。これはそもそもMVCパターンを壊すことになるので、（本来）あってはならないことです。

もうひとつの問題は、ビューやパーシャルにRubyコードを埋め込みすぎてしまうことでしょう。ロジックの一部は、ビューヘルパーやDecorator（「ビューモデル（View Model）」やプレゼンター（Presenter）と呼ばれることもあります）の中に置かれる可能性もあります。これらの例のいくつかについては今後の本シリーズ記事で見ていきますので、どうぞご期待ください。

#### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-3) コントローラの問題

Railsのコントローラもまた、実にさまざまな問題に苦しめられることがあります。そのひとつが「ファットコントローラ」と呼ばれるアンチパターンです。

先ほどのモデルも太っていましたが、こちらはある程度の減量に成功しました。しかしその作業中に、今度はコントローラが少々太ってしまいました。これは、ビジネスロジックがコントローラの内部に置かれている場合によく起きますが、ビジネスロジックの本来の置き場所は、「モデル」または「それ以外のどこか」になります。先ほど[モデルの問題](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#5-2)で触れた内容の多くは、コントローラにも当てはまります。「コードをPresenterに切り出す」「Active Recordコールバックを使う」「最後の手段として[Service Object](https://blog.appsignal.com/2020/06/17/using-service-objects-in-ruby-on-rails.html)を使う」などです。

開発者によっては、[Trailblazer](https://github.com/trailblazer/trailblazer)や[dry-transaction](https://dry-rb.org/gems/dry-transaction/)などのgemを最後の手段にすることもあります。要は、特定のトランザクションを扱うためのクラスを作るということです。コントローラからいろんなものを追い出してすっきりと保ち、追い出したものやテストは別クラスに置きます。その別クラスを何と呼ぶかは開発者によって異なり、「Service」「トランザクション」「アクション」などさまざまです。

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#6) まとめ

世の中にはさらに多くのアンチパターンがあり、さらに多くのソリューションもあります。本記事だけですべてをカバーするのは紙面と時間を使いすぎますし、今回述べたモデルやコントローラのように記事まで太ってしまうでしょう。RailsのMVCパターンのあらゆる側面を深く追求する本シリーズのフォローをぜひお願いします。それによって、皆さんも有名なアンチパターンのほとんどを扱えるようになるでしょう。第1回はパターンとアンチパターンの概要、そしてRuby on Railsフレームワークで最もよくあるアンチパターンについてご紹介しましたが、皆さんにお楽しみいただければと思います。

それでは次回お会いしましょう！

### [⚓](https://techracho.bpsinc.jp/hachi8833/2021_01_04/101446#7) お知らせ

Rubyのマジックに関する記事が公開されたらすぐ読みたい方は、[元記事末尾のフォーム](https://blog.appsignal.com/#ruby-magic)にて「Ruby Magic」ニュースレターをご購読いただければ、新着記事を見逃さずに読めるようになります。

## 関連記事

> [Railsのパターンとアンチパターン2: モデル編とマイグレーション（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_01_07/102392)

> [Railsのパターンとアンチパターン3: ビュー編（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_09_30/112127)

> [Railsのパターンとアンチパターン4: コントローラ編（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_10_05/112108)

> [Railsのパターンとアンチパターン5: 一般的な問題と、その教訓（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_10_08/112144)

> [肥大化したActiveRecordモデルをリファクタリングする7つの方法（翻訳）](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738)