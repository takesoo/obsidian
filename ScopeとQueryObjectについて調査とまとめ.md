---
Tags:
  - Ruby_on_Rails
---
## scopeはパブリックなメソッドとして振る舞うので変更に弱い。

> scopeは記述されたModelに対し**パブリックなメソッド**として振る舞います。  
> （中略）
> 
> このように**scopeは修正に対して弱く、影響範囲も広くなりがち**という特性があります。

  

## scopeはドメインの詳細を抽象化して隠すことが目的。

> scopeを使ってドメインを表現する集合を引っ張ってくるときに、どんなカラムにどんな絞り込みをかけるのかは**抽象化**することができます。これはscopeのメリットです。
> 
> つまり**どんな手続きでドメインを表現する集合を得るのかを隠蔽**することができます。

コントローラーにモデルの詳細を漏らさせない。

  

## ユースケースを表現する場合はscopeよりもQueryオブジェクトが適している。

> 上記の例はユースケースを表現していたscopeを安易に使いまわしてしまったことによる**暗黙的な結合**でした。
> 
> ユースケースとはざっくり言うと**特定のワンアクション専用の状態**のことです。
> 
> クエリがユースケースによって複雑に組み立てられる場合、scopeをまとめたscopeを新たに用意する、Modelのクラスメソッドにする、などの選択肢があります。が、どちらもパプリックアクセス可能なので別場所で呼び出される可能性があり、潜在的な結合を生む要因となります。
> 
> オススメは**クエリオブジェクトを作ってそちらにクエリ組み立てを委譲**する方法です。こうすることで暗黙的な結合を防ぎつつ、ユースケース依存の複雑なクエリを組み立てることができます。

１ユースケースにつき１クエリオブジェクトとするのが理想。

  

[

Railsのscopeのアンチパターンとその解消法 - Qiita

本記事は Classi Advent Calendar 2020 23日目の記事です。 こんにちは。@seigaです。今回はRailsのscopeのアンチパターンとその解消法について解説します。 **scope(named_scope...

![](https://cdn.qiita.com/assets/favicons/public/apple-touch-icon-ec5ba42a24ae923f16825592efdc356f.png)https://qiita.com/Seiga/items/e71e9497a395fe61102f

![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fcdn.qiita.com%2Fassets%2Fpublic%2Farticle-ogp-background-9f5428127621718a910c8b63951390ad.png?ixlib=rb-4.0.0&w=1200&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTkxNiZ0eHQ9UmFpbHMlRTMlODElQUVzY29wZSVFMyU4MSVBRSVFMyU4MiVBMiVFMyU4MyVCMyVFMyU4MyU4MSVFMyU4MyU5MSVFMyU4MiVCRiVFMyU4MyVCQyVFMyU4MyVCMyVFMyU4MSVBOCVFMyU4MSU5RCVFMyU4MSVBRSVFOCVBNyVBMyVFNiVCNiU4OCVFNiVCMyU5NSZ0eHQtY29sb3I9JTIzMjEyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTU2JnR4dC1jbGlwPWVsbGlwc2lzJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnM9MmU3MjgwZWE4YTU5Y2M5OTNkYzAwMzc4YjY5ZWY0Nzk&mark-x=142&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTYxNiZ0eHQ9JTQwU2VpZ2EmdHh0LWNvbG9yPSUyMzIxMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtYWxpZ249bGVmdCUyQ3RvcCZzPTA4Zjc1YzQyNmY1YTg4MzFiYzZmZjc3OTQzMDcxYTUy&blend-x=142&blend-y=491&blend-mode=normal&s=97aeaeef5f2057b3ee348ed78a6f71af)](https://qiita.com/Seiga/items/e71e9497a395fe61102f)

  

  

---

  

## クエリオブジェクトとは

> `ActiveRecord`リレーションで実行しなければならないクエリが複雑になったら、Query Objectパターンの利用を検討すべきです。スコープをこの目的に使うのは通常おすすめできません。
> 
> 経験から言うと、スコープが複数のカラムとやり取りする場合や、他のテーブルとJOINする場合は、Query Objectへの移行を検討すべきです。これにより、モデルに定義するスコープの数を必要最小限に減らせるという副次的効果も得られます。同様に、スコープのチェインを扱う場合は常にQuery Objectの利用を検討すべきです
> 
> [
> 
> Railsで重要なパターンpart 2: Query Object（翻訳）｜TechRacho by BPS株式会社
> 
> 概要 原著者の許諾を得て翻訳・公開いたします。 英語記事: Essential RubyOnRails patterns — part 2: Query Objects 公開日: 2017/09/18 著者: Błażej Kosmowski サイト: selleo.com 2017/10/25: 初版公開 2022/03/24: 更新 Railsで重要なパターンpart 2: Query Object（翻訳） 前回: Railsで重要なパターンpart1: Service Object Query Object（または単にQuery）パターンもまた、Ruby on Rails開発者が肥大化したActiveRec […]
> 
> ![](https://techracho.bpsinc.jp/wp-content/uploads/2017/09/cropped-techracho_official_icon-1-192x192.png)https://techracho.bpsinc.jp/hachi8833/2022_03_24/47287
> 
> ![](https://techracho.bpsinc.jp/wp-content/uploads/2017/10/rails_query_object_pattern_eyecatch2-min.png)](https://techracho.bpsinc.jp/hachi8833/2022_03_24/47287)

  

> Queryオブジェクトは「Relationに対し結合や絞り込み、ソートなどの操作を定義し、Relationを返すクラス」です。ActiveRecordモデルのscopeとして設定することで、チェーンの一部として使用できるようになります。  
> （中略）  
> Queryオブジェクトは「コントローラからクエリ操作の責務を分離し、ActiveRecordモデルと疎結合に保つため」に必要なデザインパターンです。
> 
> コントローラからクエリを操作する場合、複雑なクエリは長くなり、肥大化してしまいます。また正しいRelationが取得できているかのテストが書きづらくなります。再利用性もありません。  
> （中略）  
> Queryオブジェクトは、コントローラからクエリ操作の責務を分離し、クラス間を疎結合に保つためのデザインパターンです。
> 
> [
> 
> Railsのデザインパターン: Queryオブジェクト
> 
> RailsのデザインパターンのひとつであるQueryオブジェクトは、コントローラからActiveRecordモデルに対する絞り込みなどの操作を、ひとつの責務としてクラスに切り出すパターンです。コントローラの肥大化を防ぎ、またテストが書きやすくなります。このQueryオブジェクトについて示します。
> 
> ![](https://applis.io/favicon.svg)https://applis.io/posts/rails-design-pattern-query-objects
> 
> ![](https://applis.io/api/og?title=Rails%E3%81%AE%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%3A+Query%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88&tag=Ruby+on+Rails)](https://applis.io/posts/rails-design-pattern-query-objects)

  

コントローラからクエリ操作の責務を分離することが目的。

複雑なクエリを書く、複数のscopeを組み合わせる場合はだいたいユースケースの表現であるので、ユースケースの表現としてQueryオブジェクトを使うという理解で良さそう。

Q. Queryオブジェクトはコントローラーかscopeどちらから呼び出すのか？

Q.