---
finished reading: false
---
# TestProf: Ruby/Railsの遅いテストを診断するgem（翻訳）｜TechRacho by BPS株式会社
  #ReadItLater 
 #ReadableArticle

## articleURL
https://techracho.bpsinc.jp/hachi8833/2023_07_06/46290

## siteName
TechRacho

## date
2024-08-06

## articleContent
テストを書くことは、開発における重要なプロセスであり、RubyやRailsのコミュニティには特に当てはまります。私たちはテストでgreenが点灯するまで長時間待たされていることに気づいて、初めてテストスイートのパフォーマンスというものに関心を寄せるようになるものです。

私はテストスイートのパフォーマンスの分析に多くの時間を費やし、テストを高速化するテクニックを編み出すとともにツールを開発しました。そうしたノウハウをすべて[TestProf](https://github.com/test-prof/test-prof)という名のメタgemに盛り込みました。TestProfはRubyのテストをプロファイリングするツールボックスです。

[![test-prof/test-prof - GitHub](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/test-proftest-prof%20-%20GitHub.svg)](https://github.com/test-prof/test-prof)

### [🔗](https://techracho.bpsinc.jp/hachi8833/2023_07_06/46290#motivation) 開発の動機

**テストが遅いと生産性が低下し、貴重な時間が無駄になってしまう**

「テストスイートのパフォーマンスがどうしてそんなに重要なんだろうか？」とお思いの方もいらっしゃるでしょうから、議論を始める前にいくつかの統計情報をお見せしたいと思います。

今年初頭に、Ruby開発者にテストのパフォーマンスに関する簡単な聞き取り調査を行いました。

最初によいお知らせです。Ruby開発者のほとんどすべてがテストを書いています（正直、私は驚きませんが）。私はRubyコミュニティのこういうところが好きです。

![質問: テストを書いていますか？](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/質問%20テストを書いていますか？.png)

質問: テストを書いていますか？

調査の結果、テスト実行に10分以上かかっているケースは4分の1程度にとどまりました。かつ、半分近くは5分以内に収まっています。

![質問: テストの実行時間は？](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/質問%20テストの実行時間は？.png)

質問: テストの実行時間は？

これなら大きな心配はなさそうです。それでは、exampleが1000を超えるテストスイートに限定して聞いてみましょう。

![質問: テストスイート全体の実行時間は？（exampleが1000以上の場合）](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/質問%20テストスイート全体の実行時間は？（exampleが1000以上の場合）.png)

質問: テストスイート全体の実行時間は？（exampleが1000以上の場合）

今度はだいぶ残念な結果になりました。テストスイートの約半分は実行に10分以上を要し、ほぼ3割近くが20分以上かかっています。

ところで、私がこれまで経験した典型的なRailsアプリでは、ざっくり6000件〜10000件のexampleがありました。

もちろん、変更のたびにテストスイート全体を実行する必要などありません。通常、私が中規模の機能を手がけている場合はコミットあたり100件のexampleを実行しており、実行には1分程度しかかかりません。しかし1分といえども私のフィードバックループは結構な影響を受けますし（Justin Searlsの動画↓をご覧ください）、その間私の貴重な時間は無駄になります。

それでも私たちは、デプロイサイクル中にCIサービスですべてのテストを実行しなければなりません。テスト完了まで10分も（キューでビルドの負荷が生じれば数時間）待たされても平気ですか？私にはそうは思えません。

ビルドの並列処理化で待ち時間を軽減することもできますが、結局コストに跳ね返ります。次のグラフをご覧ください。

![質問: CIでの並行ジョブ数は？](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/質問%20CIでの並行ジョブ数は？.png)

質問: CIでの並行ジョブ数は？

たとえば現在のプロジェクトで5倍の並列処理を行っているとすると、ジョブ1つあたりの平均RSpec実行時間はexample 1250件あたり2.5分を要します。つまりEPM（examples per minute）は500になります。

最適化を行う前は、800件のexampleで4分を要しました。EPMにしてわずか200です。つまり最適化によってビルドあたり3、4分を節約できたのです。

明らかに、遅いテストはあなたの貴重な時間を奪い、生産性を低下させているのです。

### [🔗](https://techracho.bpsinc.jp/hachi8833/2023_07_06/46290#toolbox) ツールボックス

あなたがテストスイートの遅さに気づいたとしましょう。ではテストが遅い理由をどうやって明らかにしますか？

能書きは以下の動画に任せて、Rubyテストのプロファイリングツールボックスである[TestProf](https://github.com/test-prof/test-prof) gemをご紹介いたします。

TestProfはテストスイートのボトルネックを突き止め、改善方法を示してくれます。

これより、私がTestProfを使ってテストスイートの分析と改善を行ってみます。

### [🔗](https://techracho.bpsinc.jp/hachi8833/2023_07_06/46290#general) 一般的なプロファイリング

テストスイートの詳細をいきなり掘り下げるよりも、まず一般的な情報を集める方が有用なことがしばしばあります。

試しに、テストスイートについて以下の質問に答えてみてください。

1.  テストスイートのどこで時間を食っているか: コントローラか、モデルか、サービスか、ジョブか？
2.  最も時間のかかっているモジュールやメソッドはどれか

これだけでも面倒な作業ですよね。

質問1の答えが知りたい場合は、TestProfの[Tag Profiler](https://test-prof.evilmartians.io/#/profilers/tag_prof.md)を使います。これはRSpecの特定のタグ値でグループ化された統計情報を収集してくれます。RSpecはexampleに`type`タグを[自動で追加する](https://rspec.info/features/6-0/rspec-rails/directory-structure/)ので、以下のように使えます。

```
TAG_PROF=type rspec


[TEST PROF INFO] TagProf report for type

          type          time   total  %total   %time           avg

    controller     08:02.721    2822   39.04   34.29     00:00.171
       service     05:56.686    1363   18.86   25.34     00:00.261
         model     04:26.176    1711   23.67   18.91     00:00.155
           job     01:58.766     327    4.52    8.44     00:00.363
       request     01:06.716     227    3.14    4.74     00:00.293
          form     00:37.212     218    3.02    2.64     00:00.170
         query     00:19.186      75    1.04    1.36     00:00.255
        facade     00:18.415      95    1.31    1.31     00:00.193
    serializer     00:10.201      19    0.26    0.72     00:00.536
        policy     00:06.023      65    0.90    0.43     00:00.092
     presenter     00:05.593      42    0.58    0.40     00:00.133
        mailer     00:04.974      41    0.57    0.35     00:00.121
        ...
```

これで、ボトルネックを調べる際のテストのtypeを絞り込めるようになりました。

[![ruby-prof/ruby-prof - GitHub](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/ruby-profruby-prof%20-%20GitHub.svg)](https://github.com/ruby-prof/ruby-prof)  
[![tmm1/stackprof - GitHub](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/tmm1stackprof%20-%20GitHub.svg)](https://github.com/tmm1/stackprof)

[RubyProf](https://github.com/ruby-prof/ruby-prof)や[StackProf](https://github.com/tmm1/stackprof)のような一般的なRubyプロファイラを使ったことがある方もいると思いますが、TestProfは面倒な設定や改造を一切行わずにテストスイートに対して手軽に実行できます。

```
TEST_RUBY_PROF=1 bundle exec rake test

# 以下も同じ

TEST_STACK_PROF=1 rspec
```

TestProfの各種プロファイラが生成するレポートを使って、最も利用頻度の高いスタックパスを突き止めることができ、質問2.にも答えられるようになります。

残念ながら、この種のプロファイリングはリソース消費が著しく、既に遅いテストスイートの実行がますます遅くなるので、テストのごく一部に狙いを絞り込んでプロファイルを実行しなければなりません。しかしどうやって絞り込めばよいのでしょうか。実は、ランダムに絞り込めるのです。

TestProfには[特殊なパッチ](https://test-prof.evilmartians.io/#/recipes/tests_sampling)が同梱されており、RSpecのexampleグループ（またはMiniTestのスイート）をランダムに選んで実行できます。

```
SAMPLE=10 bundle exec rspec
```

後はコントローラのテストのサンプルに対してStackProfを実行し（TagProfでここが最も遅かったので）、出力結果を元に分析すればよいのです。私があるプロジェクトに対して実行した結果を以下に示します。

```
%self     calls  name
20.85       721   <Class::BCrypt::Engine>#__bc_crypt
 2.31      4690  *ActiveSupport::Notifications::Instrumenter#instrument
 1.12     47489   Arel::Visitors::Visitor#dispatch
 1.04    205208   String#to_s
 0.87    531377   Module#===
 0.87    117109  *Class#new
```

これを見ると、test環境における[Sorcery](https://github.com/Sorcery/sorcery)の暗号化設定がproduction環境と同じぐらい厳密な設定になっていることがわかります。

[![Sorcery/sorcery - GitHub](Clippings/assets/TestProf%20RubyRailsの遅いテストを診断するgem（翻訳）｜TechRacho%20by%20BPS株式会社-2024-08-06%2016-28-33/Sorcerysorcery%20-%20GitHub.svg)](https://github.com/Sorcery/sorcery)

典型的なRailsアプリの場合、レポートはほとんどの場合以下のようになるでしょう。

```
 TOTAL    (pct)     SAMPLES    (pct)     FRAME
   205  (48.6%)          96  (22.7%)     ActiveRecord::PostgreSQLAdapter#exec_no_cache
    41   (9.7%)          22   (5.2%)     ActiveModel::AttributeMethods::#define_proxy_call
    20   (4.7%)          14   (3.3%)     ActiveRecord::LazyAttributeHash#[]
```

`ActiveRecord`が随分と多くなっていて、データベースの利用量が多いことがわかります。ではこれをどうやって改善すればよいのでしょうか。続きをご覧ください。

### [🔗](https://techracho.bpsinc.jp/hachi8833/2023_07_06/46290#interaction) データベースとのやりとり

テストスイートの実行時間にデータベースが占める割合がどのぐらいあるかを把握していますか？まずは自分であたりを付けてから、TestProfであぶり出してみましょう。

私たちはRailsのInstrumentation周りを[既に拡張した](https://evilmartians.com/chronicles/fighting-the-hydra-of-n-plus-one-queries)（ActiveSupportの[Notification](http://api.rubyonrails.org/classes/ActiveSupport/Notifications.html)/[Instrumentation](http://guides.rubyonrails.org/active_support_instrumentation.html)機能）ので、基本的な概要は省略して[EventProfiler](https://test-prof.evilmartians.io/#/profilers/event_prof)を紹介します。

EventProfはテストスイート実行中にinstrumentationメトリクスを収集し、遅いグループのランキングや、特定のメトリクスに関連するexampleについての一般的な情報をレポートします。現時点では、`ActiveSupport::Notifications`のみ無設定で利用できますが、自分のソリューションへの統合は簡単なはずです。

データベース利用量の情報を取得するには、`sql.active_record`イベントを使います。この場合のレポートは次のようになります（`rspec --profile`の出力と非常に似ています）。

```
EVENT_PROF=sql.active_record rspec ...

[TEST PROF INFO] EventProf results for sql.active_record

Total time: 00:05.045
Total events: 6322

Top 5 slowest suites (by time):

MessagesController (./spec/controllers/messages_controller_spec.rb:3)–00:03.253 (4058 / 100)
UsersController (./spec/controllers/users_controller_spec.rb:3)–00:01.508 (1574 / 58)
Confirm (./spec/services/confirm_spec.rb:3)–00:01.255 (1530 / 8)
RequestJob (./spec/jobs/request_job_spec.rb:3)–00:00.311 (437 / 3)
ApplyForm (./spec/forms/apply_form_spec.rb:3)–00:00.118 (153 / 5)
```

私の現在のプロジェクトでは、DBが総時間に占める割合はおよそ20%ですが、これは既に十分最適化を行った結果です。最適化前は30%を超えていました。

どのプロジェクトでも共通でチェックに使えるような単一のメトリクス指標というものはありません。これはテストのスタイルによって大きく変わるものであり、単体テストと結合テストのどちらが多いかによって異なります。

なお私たちの場合は結合テストがほとんどだったため、20%という値は決して悪くありません（もちろんさらに改善は可能なはずですが）。

データベースがテスト時間に占める割合が高い典型的な理由とは何でしょうか。そのすべてを列挙するのは無理ですが、一部をリストアップしてみました。

-   無意味なデータ生成
-   テストの準備が重すぎる（`before`や`setup`のフック）
-   ファクトリーがカスケードしている

最初の項目は、有名な「`Model.new` vs `Model.create`問題」（またの名を「[FactoryGirlにおける](https://robots.thoughtbot.com/use-factory-girls-build-stubbed-for-a-faster-test)`build_stubbed` vs `create`問題」）です。モデルの単体テストでデータベースを叩く必要は普通ないはずなので、データベースにはアクセスしないでくださいね。

しかし既にテストコードでデータベースにアクセスしているとしたらどうでしょう。テストで永続性が不要かどうかをどうやって突き止めればよいでしょうか。そこで[Factory Doctor](https://test-prof.evilmartians.io/#/profilers/factory_doctor)の登場です。

Factory Doctorは、不要なデータ作成がいつ行われたかを通知してくれます。

```
FDOC=1 rspec

[TEST PROF INFO] FactoryDoctor report

Total (potentially) bad examples: 2
Total wasted time: 00:13.165

User (./spec/models/user_spec.rb:3)
  validates name (./spec/user_spec.rb:8)–1 record created, 00:00.114
  validates email (./spec/user_spec.rb:8)–2 records created, 00:00.514
```

Factory Doctorは残念ながら魔法使いではありません（まだ学習中です）ので、偽陽性や偽陰性が生じることもあります。

2番目の問題は、ややトリッキーな点です。次の例をご覧ください。

```
describe BeatleSearchQuery do
  # この検索機能をテストしたいので
  # exampleごとに何らかのデータが必要
  let!(:paul) { create(:beatle, name: 'Paul') }
  let!(:ringo) { create(:beatle, name: 'Ringo') }
  let!(:george) { create(:beatle, name: 'George') }
  let!(:john) { create(:beatle, name: 'John') }

  # この後15件ほどexampleが続く
end
```

「そんなのfixtureでいいじゃない」とお思いかもしれません。十数個ものモデルが毎日変更されるようなかなり大きなプロジェクトでなければ悪くない方法です。

もうひとつの方法は`before(:all)`フックでデータを1度だけ生成することです。しかしこの方法には1つ注意点があります。`before(:all)`はトランザクションの外で実行されるので、データベースを手動でクリーンアップしなければなりません。

でなければ、グループ全体を1つのトランザクションに手動で閉じ込めるというのはどうでしょうか。TestProfの`before_all`ヘルパーはまさにこれを行っているのです。

```
describe BeatleSearchQuery do
  before_all do
    @paul = create(:beatle, name: 'Paul')
    @ringo = create(:beatle, name: 'Ringo')
    @george = create(:beatle, name: 'George')
    @john = create(:beatle, name: 'John')
  end

  # この後15件ほどexampleが続く
end
```

コンテキストを別のグループ（ファイル）間でも共有したい場合は、[Any Fixture](https://test-prof.evilmartians.io/#/recipes/any_fixture)をご検討ください。これは、（ファクトリーを使って）コードからフィクスチャを生成するのに使えます。

### ファクトリーのカスケード問題

ファクトリーのカスケード問題は実にありがちなのですが、めったに対処されていないので、テストスイートが遅くなる可能性があります。言ってみれば、ファクトリー呼び出しのネストによって大量のデータが生成されるという、制御不能なプロセスなのです、TestProfはこれに対処する方法を理解していて、これについて専用の記事もありますので、ぜひお読みください↓。

> [TestProf（2） Rubyテストの遅いfactoryを診断治療する（翻訳）](https://techracho.bpsinc.jp/hachi8833/2020_09_23/96049)

### バックグラウンドジョブ

データベース以外にもさまざまなボトルネックがあります。その中からひとつを取り上げてみましょう。

テストでよく行われるのが、バックグラウンドジョブをインライン化する（`Sidekiq::Testing.inline!`など）という手法です。

通常、重たい作業はバックグランドに回しますので、すべてのジョブを実行すると実行時間が無条件に長くなります。

TestProfはバックグラウンドに要した時間のプロファイリングもサポートしています（現時点ではSidekiq限定）。以下のようにプロファイルに`sidekiq.inline`を指定するだけでできます。

```
EVENT_PROF=sidekiq.inline bundle exec rspec
```

これで、無駄になっている時間を正確に知ることができます。次はどうすればよいでしょうか。単にインラインモードをオフにすると、テストのexampleが大量に動かなくなるかもしれません。あまりに多さにすぐには修正しきれないほどです。

解決方法は、インラインをグローバルにオフにし、必要な場合のみオンにすることです。RSpecをお使いの場合は次のようにします。

```
# 共有コンテキストを追加
shared_context "sidekiq:inline", sidekiq: :inline do
  around(:each) { |ex| Sidekiq::Testing.inline!(&ex) }
end

# 必要な場合にのみ使う
it "do some bg", sidekiq: :inline do
  ...
end
```

失敗したexampleのひとつひとつにタグを付けて回らなければならないのでしょうか？TestProfならそんな必要はありません。

ツールボックスには[RSpec Stamp](https://test-prof.evilmartians.io/#/recipes/rspec_stamp)という特殊なツールが含まれており、特定のタグを自動で追加してくれます。

```
RSTAMP=sidekiq:inline rspec
```

ところで、RSpec Stampの内部ではソースファイルをパースしてタグを正しく挿入するために[Ripper](https://ruby-doc.org/stdlib-2.4.0/libdoc/ripper/rdoc/Ripper.html)を用いています。

`inline!`を`fake!`に移行する方法については[RSpec Stampのマニュアル](https://test-prof.evilmartians.io/#/recipes/rspec_stamp)をご覧ください。

---

TestProfは[GitHub](https://github.com/test-prof/test-prof)と[rubygems.org](https://rubygems.org/gems/test-prof)で公開されており、いつでもアプリに導入してテストスイートのパフォーマンス向上に役立てることができます。

本記事はTestProfの紹介のみにとどまっており、すべての機能をカバーしているわけではありません。詳しくは以下の追加リソースをどうぞ。

-   [TestProfガイド](https://test-prof.evilmartians.io/#/)
-   [RubyConfBy](https://rubyconference.by/)（注: リンク切れ）でのセッション「Run Test Run」（[動画](https://www.youtube.com/watch?v=q52n4p0wkIs)、[スライド](https://speakerdeck.com/palkan/rubyconfby-minsk-2017-run-test-run)）
    -   訳注: セッションタイトルは[Star Warsのセリフ](https://www.traileraddict.com/star-wars-4/run-luke-run "Run, Luke, Run")のもじりです
-   [テストスイートを高速化する秘訣](https://medium.com/appaloosa-store-engineering/tips-to-improve-speed-of-your-test-suite-8418b485205c)（[Benoit Tigeot](https://github.com/benoittgt)）

今年9月にモスクワで開催予定の[RailsClub](http://railsclub.ru/)でもテストのパフォーマンスについてスピーチしますので、ぜひご来場ください！

---

スタートアップをワープ速度で成長させられる地球外エンジニアたちへ告ぐ！[Evil Martiansのフォーム](https://evilmartians.com/#talk-to-us)にて待つ。

## 関連記事

> [TestProf（2） Rubyテストの遅いfactoryを診断治療する（翻訳）](https://techracho.bpsinc.jp/hachi8833/2020_09_23/96049)

> [FactoryGirlでtraitを使うとintegration test書くのが捗るという話](https://techracho.bpsinc.jp/morimorihoge/2013_08_23/12744)