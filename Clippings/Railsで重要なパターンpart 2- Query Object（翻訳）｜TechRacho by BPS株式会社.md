---
URL: https://techracho.bpsinc.jp/hachi8833/2022_03_24/47287
---
原著者の許諾を得て翻訳・公開いたします。

- 英語記事: [Essential RubyOnRails patterns — part 2: Query Objects](https://medium.com/@blazejkosmowski/essential-rubyonrails-patterns-part-2-query-objects-4b253f4f4539)
- 公開日: 2017/09/18
- 著者: [Błażej Kosmowski](https://medium.com/@blazejkosmowski)
- サイト: [selleo.com](http://selleo.com/)
- 2017/10/25: 初版公開
- 2022/03/24: 更新

## Railsで重要なパターンpart 2: Query Object（翻訳）

- 前回: [Railsで重要なパターンpart1: Service Object](https://techracho.bpsinc.jp/hachi8833/2022_03_17/46482)

Query Object（または単にQuery）パターンもまた、Ruby on Rails開発者が[肥大化したActiveRecordモデルを分割](https://techracho.bpsinc.jp/hachi8833/2021_01_07/14738)し、コントローラをスリムで読みやすくするのに非常に有用なパターンです。本記事はRuby on Railsを念頭に置いていますが、このパターンは他のフレームワーク（特にMVCベースで`ActiveRecord`パターンを適用できるもの）にも簡単に適用できます。

### どんなときにQuery Objectパターンを使うか

`ActiveRecord`リレーションで実行しなければならないクエリが複雑になったら、Query Objectパターンの利用を検討すべきです。スコープをこの目的に使うのは通常おすすめできません。

経験から言うと、スコープが複数のカラムとやり取りする場合や、他のテーブルとJOINする場合は、Query Objectへの移行を検討すべきです。これにより、モデルに定義するスコープの数を必要最小限に減らせるという副次的効果も得られます。同様に、スコープのチェインを扱う場合は常にQuery Objectの利用を検討すべきです（[関連記事](http://craftingruby.com/posts/2015/06/24/say-no-to_chained-scopes.html)）。

### Query Objectパターンを最大限に活用するための注意点

### 1. 命名規則をひとつに定めること

素晴らしいQuery Objectクラスに楽に名前を付けられるよう、基本的な命名規則をいくつか定めましょう。規則のひとつとして考えられるのは、Queryオブジェクト名の末尾に`Query`を追加することです。こうすることで今扱っているものが`ActiveRecord`の子孫ではなくQueryであることを常に意識できます。

その他に、モデル名を複数形にすることで、Queryがどのオブジェクトと協調動作するよう設計されているかを示す方法も考えられます。たとえば`RecentProjectUsersQuery`というQuery Objectは、呼び出されると`User`のリレーションを返すことが明確にわかります。

どの規則を選ぶにしても、パターンに基づいたクラスの命名法が一貫していれば、新規導入クラスの命名に迷う時間を減らせるので、メリットを得られる機会が増えます。

### 2. リレーションを返す`.call`メソッドをQuery Objectの呼び出しに使うこと

Service Objectの場合は、Service Objectを使う専用メソッドの命名方法にある程度選択の余地がありますが、対照的に、RailsでQuery Objectパターンを最大限に活用するには、リレーションオブジェクトを返す`.call`メソッドを実装すべきです。

この規則に従うことで、必要に応じてQuery Objectで簡単にスコープを構成できるようになります（[関連記事](http://craftingruby.com/posts/2015/06/29/query-objects-through-scopes.html)）。

### 3. オブジェクトなどのリレーションは常に第1引数で受け取ること

導入するQuery Objectの呼び出しでは、第1引数でリレーションを受け取るのがよい方法です。Query Objectをスコープとして利用するときに第1引数のリレーションが必須（2.の推奨事項を参照）になりますし、Query Objectをチェインできるので柔軟性も高まります。

Query Objectの使いやすさを損なわないためには、デフォルトのエントリリレーションを設定して、引数なしでもQuery Objectを利用できるようにしましょう。また、Query Objectが返すリレーションは、常にリレーションQuery Objectが提供されたときと同じ主題（テーブル）を持つことも重要です。

### 4. 追加オプションを受け取れるようにすること

追加オプション受け取りの必要性は、既存のQuery Objectや新規Query Objectの導入時にサブクラス化することである程度回避できますが、いずれQuery Objectで追加オプションを受け取る必要が生じます。

Query Objectで追加オプションを受け取れるようにしておけば、結果をどのように返すかというロジックをカスタマイズできるので、Query Objectを柔軟なフィルタとして効果的に利用できます。コードが読みにくくならないよう、追加オプションは必ずキーワード引数かハッシュとして渡し、デフォルト値も設定しておくことをおすすめします。

### 5. 読みやすいクエリメソッドを書くことに集中すること

Queryのコアロジックを`.call`メソッド自身の中に保存する場合であっても、Query Objectの別のメソッドに保存する場合であっても、常に読みやすさを心がけるべきです。他の開発者はQuery Objectの意図を確認する際にクエリメソッドを調べるので、少し手間をかけてでもクエリメソッドを読みやすくしておけば、Query Objectを活用しやすくなります。

### 6. Query Objectを名前空間でグループ化すること

プロジェクトの複雑さや、`ActiveRecord`をどの程度利用するかによって多少異なりますが、いずれQuery Objectはどんどん増えていきます。

コードを整理するよい方法のひとつは、互いによく似たQuery Objectを名前空間でグループ化することです。Queryが扱うモデルの名前でグループ化しても構いませんし、十分な理由付けがなされていれば何を使っても構いません。これまでと同様、Query Objectのグループ化方法も1つに決めておくことで、新規導入するクラスの適切な配置が楽に決まります。

Query Objectをすべて_app/queries_ディレクトリに保存する方法もおすすめです。

### 7. すべてのメソッドを`.call`の結果に委譲することも検討すること

Query Object用の`method_missing`を実装して全メソッドを`.call`メソッドの結果に委譲する方法も考えられます。この方法の場合、Query Objectは単に通常のリレーションとして用いられます（つまり `RecentProjectUsersQuery.call.where(first_name: “Tony”)`ではなく`RecentProjectUsersQuery.where(first_name: “Tony”)`になります）。

しかし、この方法を選ぶ際には、メタプログラミングと同様に十分な検討と理由付けを行うべきです。

### まとめ

Query Objectパターンは、実装の複雑なクエリ/リレーション/スコープを抽象化できるシンプルなパターンであり、テストも簡単になります。上述のシンプルな規則に従うことで、可読性や柔軟性を失わずにこのパターンを簡単に利用できるようになります。開発者自身はもちろん、何より将来そのコードを使う他の開発者にとってメリットになります。

そのようなQuery Objectの実装例を以下に示します。

```
module Users  class WithRecentlyCreatedProjectQuery    DEFAULT_RANGE = 2.days    def self.call(relation = User.all, time_range: DEFAULT_RANGE)      relation.        joins(:projects).        where('projects.created_at > ?', time_range.ago).        distinct    end  endend
```

Query Objectパターンをシンプルに抽象化したい場合は、[rails-patterns](https://github.com/Selleo/pattern) gemが提供するラッパーの導入をご検討ください。

- 前回: [Railsで重要なパターンpart1: Service Object](https://techracho.bpsinc.jp/hachi8833/2022_03_17/46482)

## 関連記事

[Railsで重要なパターンpart 1: Service Object（翻訳）](https://techracho.bpsinc.jp/hachi8833/2022_03_17/46482)

概要 原著者の許諾を得て翻訳・公開いたします。 英語記事: Essential RubyOnRails patterns — part 1: Service Objects 公開日: 2017/06/13 著者: Błażej Kosmowski サイト: selleo.com パターンの種別は原則として英語表記にしました。 2017/10/16: 初版公開 2022/03/17: 更新 Railsで重要なパターンpart 1: Service Object（翻訳） Service Object（単にServiceと呼ばれることもあります）は、肥大化したActiveRecordモデルを分割し↓、コントローラをスリ … [続きを読む Railsで重要なパターンpart 1: Service Object（翻訳）](https://techracho.bpsinc.jp/hachi8833/2022_03_17/46482)