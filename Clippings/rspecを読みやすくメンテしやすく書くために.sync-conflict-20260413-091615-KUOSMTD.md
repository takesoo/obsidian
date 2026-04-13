---
Created: Invalid date
URL: https://zenn.dev/yuji_developer/articles/52cc0e356b3748
---
## はじめに

読みやすくメンテナンスしやすいRSpecを書けていますか？

RSpecはというかRubyはというか柔軟なので色々な書き方ができてしまいます。  
ある程度の規模のテストコードでは、油断するとどこで定義されている `let` なのかわからないものが登場したり、なぜか作られる（あるいは作られない）謎のレコードでテストが失敗したり、そういった辛い目にあったりするのではないでしょうか。

僕がRSpecを書くときに意識していることをまとめてみました。  
これを実践するようになってつらい現象にあうことはずいぶんと減り、ずいぶんと読みやすくなったんじゃないかなと思っています。  
※効果には個人差があります。

Ruby on Railsを使ったアプリケーションのテスト向けですがRuby on Rails以外でも使えると思います。

主に以下の影響を強く受けています。

RSpecとセットで使われることが多いFactoryBotでのFactoryの書き方についてはここではあまり触れません。いつか別の記事で書くかもしれません。

## 1. `describe` / `context` / `it` の説明を明確に書く

`describe` にはテスト対象の「クラス」や「メソッド」などを書きます。`context` にはテストしようとしている状況「ログイン中のユーザーの場合」などを書きます。`it` は期待する結果を書きます。このとき「正しいこと」などのような表現を避けます。「正しいとはどういう状況か」を書きます。

`it` の説明を省略しがちですが単純なワンライナー以外は避けましょう。複数行になる場合に `it do` とせず `it "〜" do` のように説明を書きましょう。

`describe` / `context` / `it` の説明を読むことでテストの概要がわかるのが理想的です。  
なんのテストをしているかわかりやすく説明しましょう。

```
describe "\#full_name" do  context "last_nameのみ設定されている場合"    it "last_nameを返すこと" do      # ...    end  end  context "first_nameのみ設定されている場合"    it "first_nameを返すこと" do      # ...    end  end  context "last_nameとfirst_nameが設定されている場合"    it "last_nameとfirst_nameをスペース区切りで返すこと" do      # ...    end  endend
```

## 2. `context` を対称にする

`it` と `context` を同じ階層に書くのを避けます。

よくあるのは正常なケースの `it` と異常ケースの `context` が同じ階層に並んでいる↓のような状況です。

```
it "..." do # 正常ケース  # 正常な場合の検証endcontext "..." do # 異常ケース  it "..." do    # 異常な場合の検証  endend
```

正常ケースという暗黙の `context` に頼るのではなく、↓のように明確に `context` を分けます。

```
context "..." do  # 正常ケース  it "..." do    # 正常な場合の検証  endendcontext "..." do # 異常ケース  it "..." do    # 異常な場合の検証  endend
```

暗黙の `context` に頼ることによってケースによって参照しない `let` が必要になったり `let` の上書きが必要になったりします。

## 3. `let!` のみを使う

`let` と `let!` は使い分けしません。  
原則として `let!` のみを使います。

`let` は遅延評価されるため実行タイミングの把握が難しくなります。また、それによる意図せぬレコード作成タイミングの変化でテストが壊れやすくなります。

`let!` にすると無駄なレコードが作成される、という反論もあるかもしれません。  
そいった状況になるのは `describe` や `context` の分割が足りないか、 `let!` の定義位置がおかしいです。`let!` は必要になる箇所のみで定義するようにしましょう。  
使われるか使われないかが一目でわからないものが定義されていると無駄に脳のメモリーを消費します。

`let` を使って良いケースは遅延評価でなければ実現できない場合のみです。  
（そんなケースに出会ったことはないし思い付きませんでした。）

## 4. `let` / `let!` の上書きをしない

`let` や `let!` の上書きは原則禁止です。`let` や `let!` を上書きすると定義箇所を追うのが大変になります。  
上書きしたくなるのは `describe` や `context` の分け方の問題であることがほとんどです。  
上書きするよりコピペすることを選びます。  
テストコードにおいて過度なDRYは不要だと思っています。

## 5. テストコードで参照しないものは `let!` を使わない

セットアップも含めテストで参照しないがレコードとして存在していて欲しいものを作成する場合は `let!` ではなく `before` を使います。`let!` で名前がついてしまうとそれだけで脳の余分なメモリーを消費します。

## 6. ローカル変数を使う

`let` / `let!` である必要がないものはローカル変数を使います。  
変数のスコープを狭めましょう。

全てを `let!` で定義する必要はないです。

```
let!(:post) do  user = create(:user) # let!(:user) { ... }としがち  create(:post, user: user)end
```

## 7. `it` を必要以上に分けない

↓のような1 expect/exampleのようなことは目指しません。

```
describe "\#publish" do  let!(:post) { create(:post, :draft) }  it "ステータスがpublishedになること" do    post.publish    expect(post.status).to eq :published  end  it "published_atが設定されること" do    post.publish    expect(post.published_at).to be_present  endend
```

よほど観点が違う場合以外は1つのexample（`it`）内で全て検証します。（そもそもそんなに観点が違うなら `describe` を分けることを検討した方が良い）`aggregate_failures` を有効にしておくことでどの `expect` に問題があるかわかります。  
個人的な理想では1 example/contextで、ある状況のテストは1つのexampleを見れば何が検証されているかわかるというものです。

```
describe "\#publish" do  let!(:post) { create(:post, :draft) }  it "ステータスがpublishedになりpublished_atが設定されること" do    post.publish    expect(post.status).to eq :published    expect(post.published_at).to be_present  endend
```

## 8. `describe` の外にテストデータを置かない

全テストケースで参照している場合を除いて `describe` の外にはテストデータを置きません。  
スコープは狭めましょう。参照しない `let` や `let` の上書きの元になります。  
全テストケースで参照している場合でも各 `describe` 毎に定義するのを推奨します。前提条件の違うテストケースが追加された場合に意図せず参照しない `let` や `let` の上書きが起きてしまうことになりがちがだからです。

```
let!(:foo) {} # 原則ここには置かないdescribe "\#a" do  let!(:bar) {} # aとbで必要なものはそれぞれのdescribeで定義するenddescribe "\#b" doenddescribe "\#c" do  let!(:bar) {} # aとbで必要なものはそれぞれのdescribeで定義するend
```

## 9. 分岐やループを書かない

テストの定義をループで作ったり、example内で条件分岐したりを避けましょう。

大体同じ結果になるけどステータスだけ違うテストをループして作ったらDRYになって良いよね？ということでしょうか。

```
%i[draft published].each do |status|  context "#{status}ステータスの場合" do    subject { post.publish }    let!(:post) { create(:post, status: status) }    it { is_expected.to eq true }  endend
```

これは良くないです。過剰なDRYは避けましょう。  
今はたまたま同じでも将来もそうとは限りませんし、普通に読みづらく可読性を落としています。  
以下のように書きましょう。

```
context "draftステータスの場合" do  subject { post.publish }  let!(:post) { create(:post, status: :draft) }  it { is_expected.to eq true }endcontext "publishedステータスの場合" do  subject { post.publish }  let!(:post) { create(:post, status: :published) }  it { is_expected.to eq true }end
```

以下のようなループも避けます。  
これは `all` マッチャーを使うと良いでしょう。

```
it "..." do  posts.each do |post|    # 各postの検証  endend
```

### 10. `subject` を動詞的に扱わない

`subject` は名詞です。（動詞的用法もありますが目的語を伴う他動詞です）  
動詞的にメソッド呼び出しのように使うのは避けましょう。

```
it "..." do  subject  expect(...).to ...end
```

動詞的な使い方をしたいのであれば `subject` ではなくメソッドを定義して呼び出す方が良いと思います。

### 11. `subject` からメソッドチェーンしない

`subject` が返すレコードのカラムを参照したいような場合は名前付き `subject` を使うと良いでしょう。`have_attributes` マッチャーを使うのも良いと思います。

```
subject { post }it "..." do  expect(subject.status).to eq :published # こういうことをしないend
```

## 12. テストデータ作成時にテストで着目する値以外を直接指定しない

FactoryBotの `factory` や `trait` を使いテストに無関係な値を指定する必要をなくしましょう。  
どの値の変化に着目しているのかがわかりやすくなり認知コストが下がります。

```
describe "\#full_name" do  let!(:user) do    # birthdayのようなテストに関係ない値を指定しないようにする    create(:user, birthday: "2000-01-01", first_name: "めい", last_name: "せい")  end  it "last_nameとfirst_nameをスペース区切りで返すこと" do    expect(user.full_name).to eq "せい めい"  end end
```

## 13. テストデータを `update` などで操作しない

テストデータの操作できる限り `factory` や `trait` の指定で行い、作成したデータを `update` などで変更するのを避けます。  
テストデータの最終的な状態がわかりにくくるなるためです。  
（歴史あるアプリケーションだと仕方がない時もあります……）

## 14. テストデータの作成にアプリケーションロジックを使わない

テストデータの作成にアプリケーションロジックを使うのを避けます。  
ロジックのバグや変更で意図しないデータになったりするためです。  
（歴史あるアプリケーションだと仕方がない時もあります……）

## 15. `shared_examples` を複数ファイルで共有を避ける

`shared_exmaples` を有効に使うのはとても難しいです。  
スコープが広がれば広がるほどうまく動くものを作るのが難しくなります。また、ファイルが分かれることで認知コストがとても高くなります。  
本当に必要かを良く考えて使いましょう。大抵の場合はDRYになるメリットよりも理解が難しくなるデメリットの方が大きいと思います。

## 16. `allow_any_instance_of` / `expect_any_instance_of` の使用を避ける

`allow_any_instance_of` / `expect_any_instance_of` の使用を避けましょう。  
本当にあるクラスのインスタンスのメソッド全てがスタブ/モックになって良いのでしょうか？  
テスト対象が広すぎたりテストしやすい設計になっていないのではないでしょうか？

## おわりに

いかがだったでしょうか。  
まとめると概ね以下を守ることで読みやすくメンテナスしやすものになると思っています。

- `describe` / `context` / `it` の説明をサボらない
    - 説明は明確に
    - `context` の対称性を守る
- 変数のスコープは最小限に
    - 参照しない `let` や `let` の上書きを避ける
    - `before` やローカル変数を活用する
- 過度なDRYを追い求めない
    - コピペを必要以上に恐れない
    - そもそもDRYとはコードを共通化することではなく意図を共通化すること
- テストデータは `factory` で作った時点で作ったものにする
    - テストに関係ない値を指定しない
    - テストデータを作るためにテストデータを更新しない

皆さんはどんなことを意識しているでしょうか。  
これを気をつけるのがオススメなどあれば教えていただけると幸いです。

※全てにサンプルコードを書こうと思いましたが挫折しました……。

### Discussion