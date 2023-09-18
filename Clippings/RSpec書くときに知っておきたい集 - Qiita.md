---
Created: Invalid date
URL: https://qiita.com/yokoto/items/c7973a4b222fe19a657a
---
## GitHub

## ドキュメント

## リファレンス用途

## パターン集

## スタイルガイド

## テスト用語

- TDD
    - Red/Green/Refactor = 失敗/成功/リファクタリング
        - RSpecを先に書き、テストが通るコードを書き、その後リファクリングするような開発の流れ。
- BDD
    - Given/When/Then = 状態/振舞/変化
- AAA
    - Arrange/Act/Assert = 準備/実行/検証
- 単体テスト（Unit test）
    - ソフトウェアの単体のモジュールのテスト(例: クラスやメソッドなど)
    - RSpec では Model Spec などに相当する。
- 結合テスト（Integration test）
    - ソフトウェアの複数のモジュールのテスト(例: コントローラーのアクションやAPIなど)
    - RSpec では Request Spec に相当する。
- E2Eテスト
    - 利用者によるブラウザ操作のテスト
    - RSpec では System Spec に相当する。
- スタブ = テスト対象への間接入力を提供する。
- モック = テスト対象からの間接出力を検証する。
- 以下例。（実際には、テスト環境からは叩けないAPIのエンドポイントへのリクエストとレスポンスをモック化することが多い気がするので、思いついたら書き直す。）

```
context 'postがuserを返すこと' do  user = double('User') # これがスタブ  post = Post.new  allow(post).to receive(:user).and_return(user) # これがモック  let!(:user1) { create(:user) }  it { expect(post.user).to eq user1 }end
```

### flakey test（ランダム落ち）

- 多くの開発現場ではCIパイプラインにテストが組み込まれていることが多いが、ランダムに失敗する時と成功する時があるようなテストが出現する場合があり、このようなテストを **flakey test** と呼ぶ。
- 発生ケースをざっくり書くと、E2EのテストでDOM要素が現れるのを待つことができていないことが原因であることが多い。
- flakey test対応のアンチパターン
    - `sleep 1` などでの対処。
- ベストプラクティス
    - JavaScriptによるDOM操作が原因である場合は、`wait_for_ajax` のようなメソッドを作成・使用する。

## テストコードのないRailsアプリケーションにRSpecを導入する時の個人的方針

- とりあえず System Spec 書いておけば無いよりはマシになる。
    - テストが重くなるので、System Spec から切り出せそうな処理はそれぞれの Spec として書いた方がいい。
    - バックエンドのみのAPIモードとしてRailsを使用している場合は、この部分はフロントエンド側でJestなどを使用してテストが書かれていたりするかも。
- API に対しては Request Spec を書いて、Railsアプリケーションの規模やアーキテクチャによって Model Spec やら Form Spec , Service Spec , Mailer Spec などを書くと良いと思う。

※ 要はこれが言いたかった↓

Why not register and get more from Qiita?

1. We will deliver articles that match youBy following users and tags, you can catch up information on technical fields that you are interested in as a whole
2. you can read useful information later efficientlyBy "stocking" the articles you like, you can search right away

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fyokoto%2Fitems%2Fc7973a4b222fe19a657a&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fyokoto%2Fitems%2Fc7973a4b222fe19a657a&realm=qiita)