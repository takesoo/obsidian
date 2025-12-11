---
AI summary: Ruby on Railsのデザインパターンを導入することで、Fat ModelやFat Controller、Fat Viewなどの設計上の問題を防ぐことができます。デザインパターンの導入にはオブジェクト指向設計の原則を忠実に守る必要があります。代表的なデザインパターンには、Decorator、Delivery、Form、Interactor、Policy、Query、Validator、Value、View Componentがあります。
URL: https://applis.io/posts/rails-design-patterns
---
[![](https://applis.io/api/og?title=Ruby+on+Rails%E3%81%AE%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%BE%E3%81%A8%E3%82%81&tag=Ruby+on+Rails)](https://applis.io/api/og?title=Ruby+on+Rails%E3%81%AE%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%BE%E3%81%A8%E3%82%81&tag=Ruby+on+Rails)

---

この記事は[Rails Advent Calendar 2020](https://qiita.com/advent-calendar/2020/rails)の24日目の記事です。今月中旬に見てみたら24日だけ空いていたので参加を申し込みました。よろしくお願いいたします。

Ruby on Railsアプリケーションのディレクトリ構成としては、大きくmodelsとcontrollers、viewsがあります。ここにコードを書いていくことになります。実装が進むにしたがって問題になりやすいのが、Fat ModelやFat Controllerといったコードの肥大化に関する問題です。これは適切にデザインパターンを導入することで防ぐことができます。

この記事では、Railsにおけるデザインパターンとはなにか、その必要性やデザインパターンの一覧について書いていきます。なお、Railsアプリケーションの開発についてより詳しく学びたいときに参考になる本を次の記事でまとめています。あわせてご覧ください。

[【2023年】Ruby on Railsの本でおすすめの3冊を紹介します2023-4-2](https://applis.io/posts/ruby-on-rails-books)

Hiroki Zenigami

テクニカルライター。元エンジニア。共著で「現場で使えるRuby on Rails 5」を書きました。プログラミング教室を作るのが目標です。

[Follow @zenizh](https://twitter.com/zenizh)

スポンサーリンク

## [Ruby on Railsにおけるデザインパターンとは](https://applis.io/posts/rails-design-patterns#ruby-on-rails%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%A8%E3%81%AF)

Railsにおけるデザインパターンとは、モデルやコントローラ、ビューに頻出する実装パターンを、オブジェクト設計の原則にもとづいて抽象化したパターンのことです。デザインパターンにしたがうことで、設計上の問題を防ぐことができます。

### [例：Formオブジェクト](https://applis.io/posts/rails-design-patterns#%E4%BE%8B%EF%BC%9Aform%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

たとえばFormオブジェクトというデザインパターンがあります。これはユーザーからの入力を整形・検証して永続化するという役割を持ちます。この処理はコントローラに書かれがちですが、このことはFat Controllerにつながりやすいです。

Formオブジェクトを導入することで、Fat Controllerの問題を防げます。またFormオブジェクト単体として再利用性が上がったり、テストが書きやすくなるといった利点もあります。

## [なぜデザインパターンが必要か](https://applis.io/posts/rails-design-patterns#%E3%81%AA%E3%81%9C%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%8C%E5%BF%85%E8%A6%81%E3%81%8B)

デザインパターンは、アプリケーションが大きくなるにしたがって起こりがちな設計上の問題を防ぐために必要になります。

代表的な問題がFat ModelやFat Controller、Fat Viewです。Railsはデフォルトでモデルやコントローラ、ビューを用意しています。用意されたこのファイル群だけにコードを記述していくと、オブジェクト指向設計の原則を守るのは難しいです。つまりコードが肥大化してしまいます。

こうなると拡張性や再利用性がなく、またテストも書きづらくなります。これを防ぐために、オブジェクト指向設計の原則に基づいてクラスを分割していく必要があります。

このクラス分割について、これまで開発者によって頻出パターンを抽象化されたのがデザインパターンです。Railsアプリケーションにデザインパターンを導入することで、Fat Modelをはじめとする設計上の問題を防ぐことができます。

スポンサーリンク

## [前提条件](https://applis.io/posts/rails-design-patterns#%E5%89%8D%E6%8F%90%E6%9D%A1%E4%BB%B6)

まず、デザインパターンを導入するときの前提として、オブジェクト指向設計の原則を忠実に守る必要があります。たとえば[SOLIDの原則](https://applis.io/tags/software-design)などです。原則を守りつつ、まずはRailsが用意したモデルやコントローラ、ビューのレイヤーにコードを書いていきます。このとき純粋はRubyオブジェクトを積極的に活用します。

こうしてコードを書いていた結果、パターンが現れたときに適切なデザインパターンの導入を検討します。デザインパターンの導入ありきにならず、まずはオブジェクト指向設計の原則と向きあうことが前提となります。

これについては[@joker1007](https://twitter.com/joker1007)氏の「[俺が悪かった。素直に間違いを認めるから、もうサービスクラスとか作るのは止めてくれ](https://qiita.com/joker1007/items/25de535cd8bb2857a685)」でも詳しく書かれていますのであわせてお読みください。

## [Railsのデザインパターン一覧](https://applis.io/posts/rails-design-patterns#rails%E3%81%AE%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E4%B8%80%E8%A6%A7)

Railsのデザインパターンについて、各責務とディレクトリ名、関連Gemがあればそれを書いています。このブログに別途記事を書いているパターンについては、記事へのリンクも貼っています。

- [Decoratorオブジェクト](https://applis.io/posts/rails-design-patterns#decorator%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Delivryオブジェクト](https://applis.io/posts/rails-design-patterns#delivery%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Formオブジェクト](https://applis.io/posts/rails-design-patterns#form%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Interactorオブジェクト](https://applis.io/posts/rails-design-patterns#interactor%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Policyオブジェクト](https://applis.io/posts/rails-design-patterns#policy%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Queryオブジェクト](https://applis.io/posts/rails-design-patterns#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Validatorオブジェクト](https://applis.io/posts/rails-design-patterns#validator%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [Valueオブジェクト](https://applis.io/posts/rails-design-patterns#value%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [View Componentオブジェクト](https://applis.io/posts/rails-design-patterns#view-component%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

### [Decoratorオブジェクト](https://applis.io/posts/rails-design-patterns#decorator%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

モデルに対するビューのロジックをカプセル化する責務をもちます。ビューにはモデルに関連するロジックがよく登場しますが、ビューからモデルにアクセスすると肥大化の原因になり、また再利用性がなくテストも書きづらくなります。Decoratorオブジェクトでこの問題を防ぐことができます。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/decorators|
|関連するGem|[active_decorator](https://github.com/amatsuda/active_decorator)|

### [Deliveryオブジェクト](https://applis.io/posts/rails-design-patterns#delivery%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

通知に関するロジックをカプセル化する責務をもちます。RailsにはデフォルトでActionMailerというしくみがあります。ただ、最近はメール以外にもSlackやLINEなどのチャットツールに通知することもよくあります。こういった通知に関する処理をラップするのがDeliveryオブジェクトです。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/deliveries|
|関連するGem|[active_delivery](https://github.com/palkan/active_delivery)|

スポンサーリンク

### [Formオブジェクト](https://applis.io/posts/rails-design-patterns#form%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ユーザーからの入力を整形・検証して永続化する責務をもちます。ユーザーからの入力を処理するのはコントローラの役割です。ただ、複雑な処理や複数の場所で行われる処理をコントローラに書くと、コードの肥大化といった問題の原因になります。Formオブジェクトで、この処理をカプセル化することができます。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/forms|
|関連するGem|-|
|関連記事|[Formオブジェクト](https://applis.io/posts/rails-design-pattern-form-objects)|

### [Interactorオブジェクト](https://applis.io/posts/rails-design-patterns#interactor%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ビジネスロジックをカプセル化する責務をもちます。Interactorオブジェクトは、アプリケーションの一つのみのビジネスロジックをもちます。ビジネスロジックを一つしかもたないため、再利用性があり、テストも書きやすいです。二つ以上の処理はInteractorオブジェクトを組み合わせることで実現します。

同じような役割としてServiceオブジェクトがあります。ただServiceオブジェクトは定義があいまいで、オブジェクト指向設計の原則が守られづらいという問題があります。Interactorは役割が明確で、またGemによるルールのおかげもあり設計の原則を守りやすいという利点があります。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/interactors|
|関連するGem|[interactor](https://github.com/collectiveidea/interactor)|
|関連記事|[Interactorオブジェクト](https://applis.io/posts/rails-design-pattern-interactor-objects)|

### [Policyオブジェクト](https://applis.io/posts/rails-design-patterns#policy%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ビジネスルールをカプセル化する責務をもちます。たとえばユーザーの役割に応じて処理を実行する権限をもつかどうかを判断したりします。Policyオブジェクトがないと、コントローラやビューがルールに関するロジックであふれてしまうことになります。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/policies|
|関連するGem|[pundit](https://github.com/varvet/pundit)|

### [Queryオブジェクト](https://applis.io/posts/rails-design-patterns#query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ActiveRecord::Relationに対して結合や絞り込み、ソートなどの操作を行い、Relationを返す責務をもちます。Relationに対する複雑な操作や再利用性のある操作を分割することで、再利用ができるなどの利点を得られます。ActiveRecord::Base継承クラスのスコープ経由で呼び出すことで、依存関係が明確になり、また返却されるクラスも自明になるという利点があります。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/queries|
|関連するGem|-|
|関連記事|[Queryオブジェクト](https://applis.io/posts/rails-design-pattern-query-objects)|

スポンサーリンク

### [Validatorオブジェクト](https://applis.io/posts/rails-design-patterns#validator%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ActiveRecord::Base継承クラスのレコードを検証する責務をもちます。複数の場所で利用される検証ロジックを書くことで、再利用性などの利点を得られます。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/validators|
|関連するGem|-|

### [Valueオブジェクト](https://applis.io/posts/rails-design-patterns#value%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

値オブジェクトをカプセル化する責務をもちます。住所やメールアドレス、あるいは商品のレビューにおける星の数といった値オブジェクトを書くことで、再利用性などの利点を得られます。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/values|
|関連するGem|-|
|関連記事|[Valueオブジェクト](https://applis.io/posts/rails-design-pattern-value-objects)|

### [View Componentオブジェクト](https://applis.io/posts/rails-design-patterns#view-component%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)

ビューをコンポーネント単位でカプセル化する責務をもちます。再利用性のあるビューを、ビジネスロジックごとカプセル化することで、再利用性などの面で利点を得られます。

|   |   |
|---|---|
|項目|値|
|ディレクトリ|app/view_components|
|関連するGem|[view_component](https://github.com/github/view_component)|

## [まとめ](https://applis.io/posts/rails-design-patterns#%E3%81%BE%E3%81%A8%E3%82%81)

RailsアプリケーションのFat ModelやFat Controllerといった設計上の問題は、デザインパターンを導入することで防ぐことができます。ただ、その前提としてオブジェクト指向設計について学び、その原則にしたがって設計する必要があります。

原則を守った上でデザインパターンを導入することで、アプリケーションが大きくなっても秩序のあるコードベースを保つことができます。

### [あわせて読みたい](https://applis.io/posts/rails-design-patterns#%E3%81%82%E3%82%8F%E3%81%9B%E3%81%A6%E8%AA%AD%E3%81%BF%E3%81%9F%E3%81%84)