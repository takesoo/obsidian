---
category: "[[Clippings]]"
author: "[[弥生開発者ブログ]]"
title: Railsの複数DB機能で負荷を分散する - 弥生開発者ブログ
source: https://tech-blog.yayoi-kk.co.jp/entry/2020/10/09/110000
clipped: 2023-09-12
published: 2020-10-09
tags:
  - clippings
  - Ruby_on_Rails
---

こんにちは。弥生で Misoca を開発している小坂と申します。インターネットには [kosappi](https://twitter.com/kosappi_) という名前で存在しています。

前回ご紹介した [みんなのコンピュータサイエンス](https://tech.misoca.jp/entry/2020/09/09/110000) は読んでいただけたでしょうか？

9月末で事業年度が終わる会社は多いかと思います。みなさんは無事に10月を迎えることはできましたか？私は有給休暇の日数が付与されて、とても良い気分です 🏝

今回は、Rails の複数 DB 機能を利用して9月末の高負荷を乗り切った話を紹介いたします。

### 🔥 月末の高負荷

Misoca は請求書作成ソフトということもあり、月末にアクセスが増加します。

ユーザの増加や、機能が充実したことにより、DB への負荷も増加しています。８月末の負荷は DB の限界に近い値でした。 特に、文書の一覧や検索などの参照系のクエリの比重が高く、機能の充実によってクエリ自体も重いものになっており、問題になっています。

９月末は事業年度が終わるユーザも多く、８月末よりも負荷が高くなり、このままでは DB の処理能力の限界を超えてしまう事態が予想されました。

DB への負荷を改善する方法としては、下記のようなものがあると思います。

-   重いクエリを見直して軽くする（SQLの改善）
-   DB インスタンスの強化（スケールアップ）
-   DB インスタンスを増やして負荷を分散させる（スケールアウト）

今回は参照系のクエリが問題になっていることもあるので、読み込み専用の DB インスタンス（リードレプリカ）を増やして、一部の重いクエリをそちらに向けることにしました。

### 🛠 Active Record による複数の DB 利用

Misoca は Ruby on Rails の上で開発されています。

Rails は 6 にバージョンアップした際に、複数の DB 利用をサポートしました。Rails 5 でも、追加の gem を使えば実現できたのですが、Rails 6 からは標準機能として提供されます。

参考 : [Active Record で複数のデータベース利用](https://railsguides.jp/active_record_multiple_databases.html)

今回はこの機能を利用して、一部のクエリをリードレプリカに向けました。

#### 一部のクエリをリードレプリカに向ける

まず DB を追加します。

事前に AWS の RDS で、普段使っているインスタンスをレプリケーションするインスタンスを作りました。 レプリケーションの遅延が発生しますが、これは 1ms 以内と示されていたので、今回は問題にならないと判断しています。

メインのDBを primary、リードレプリカを replica という名前にしています。 Rails が DB を判断するために、レプリカとして利用する DB には `replica: true` を書く必要があります。

production:
  primary: &primary
    <<: \*default
    host: <%= ENV\['PRIMARY\_HOST'\]  %>
  replica:
    <<: \*primary
    host: <%= ENV\['REPLICA\_HOST'\]  %>
    replica: true

そして全てのモデルで上記の DB が利用できるようにします。 今回はロールが `writing` の場合は `primary` を、 `reading` の場合は `replica` をそれぞれ利用します。

class ApplicationRecord < ActiveRecord::Base
  self.abstract\_class = true

  connects\_to database: { writing: :primary, reading: :replica }
end

最後に、向き先を変えたいクエリを実行している箇所を、下記のようにブロックに含めればOKです。このブロックの外では必ず `role: :writing` でクエリが実行されます。 `role` 以外にも `database` を直接指定することもできますが、こちらは非推奨となっているので気をつけてください。

ActiveRecord::Base.connected\_to(role: :reading) do
    
end

今回は手動でロールを変更しましたが、自動的に切り替えることも可能です。 書き込むクエリの場合は primary、読み込みクエリの場合は replica を使う、といった振り分けを書くことができます。

参考 : [Active Record で複数のデータベース利用#コネクションの自動切り替えを有効にする](https://railsguides.jp/active_record_multiple_databases.html#%E3%82%B3%E3%83%8D%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E8%87%AA%E5%8B%95%E5%88%87%E3%82%8A%E6%9B%BF%E3%81%88%E3%82%92%E6%9C%89%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B)

### 📉 結果

以上の対策を施した上で、無事に９月末を乗り越えることができました。

DB の負荷はどうなっていたんでしょうか？

primary DB の IOPS [1](#fn:1) を見てみましょう。オレンジが READ、ブルーが WRITE の IOPS です。

リードレプリカの利用開始時期（9/15ごろ）から、READ の IOPS が下がっていることがわかります。 ８月末と９月末を比較すると、かなり負荷を減らすことができました。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/mizukmb/20221017/20221017145514.png)

今回は９月末の高負荷を、Rails の複数 DB 機能で乗り切ることができました。 今後は、２台に増えた DB のスペックを見直したり、他のクエリをリードレプリカに向けることを検討する予定です。

### 🎺 宣伝

Misoca 開発チームでは、Rails の新機能を使って課題を解決したいエンジニアを募集しています。