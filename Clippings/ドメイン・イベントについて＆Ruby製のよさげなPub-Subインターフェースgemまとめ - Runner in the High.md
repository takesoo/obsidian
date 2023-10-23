---
category: "[[Clippings]]"
author: "[[Runner in the High]]"
title: "ドメイン・イベントについて＆Ruby製のよさげなPub/Subインターフェースgemまとめ - Runner in the High"
source: https://izumisy.work/entry/2020/02/22/152900
clipped: 2023-10-23
published: 2020-02-22
topics: 
tags: [clippings]
---

[Ruby](http://d.hatena.ne.jp/keyword/Ruby)で特に[Rails](http://d.hatena.ne.jp/keyword/Rails)を使う際に「特定の[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)の変化によって別の処理の実行をトリガする」みたいなケースでは大抵の場合コールバックが使われる。 しかし、ぶっちゃけた話コールバックはかなり結合度の高いコードになってしまいがちで、実装的にスケールさせるためには[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)・イベントを使うほうが健全であると言える。 [martinfowler.com](https://martinfowler.com/eaaDev/DomainEvent.html)

以下の記事は[ActiveRecord](http://d.hatena.ne.jp/keyword/ActiveRecord)のコールバックがどのようなときによろしくない感じになるかを説明しており、非常に参考になる。一言で言うと、コールバックを使うことモデル自体に副作用が埋め込まれてしまう。一方で[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)・イベントを使うことで、副作用がなにをするものなのか（メールを送る、外部のmBaaSを更新する、モニタリングサーバへメトリクスを送信する、等）を意識しないでよくなり、[疎結合](http://d.hatena.ne.jp/keyword/%C1%C2%B7%EB%B9%E7)になる。 [techracho.bpsinc.jp](https://techracho.bpsinc.jp/hachi8833/2018_08_03/59155)

さて、[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)・イベントの仕組みを実装する際、もちろんオンメモリなオブザーバ・パターンの仕組みを実装してもよいが、それは開発環境でしか使えない。プロダクションだと大抵APサーバは複数台構成のはずなので、Redisあたりにキューして複数の異なるAPサーバ上のサブスクライバがイベントを拾えるようにしたい。こうしておけばQueue-based Load Levellingもできるようになるので負荷分散もできてスケールする。

もっというなら[GCP](http://d.hatena.ne.jp/keyword/GCP)のときはCloud Pub/Subで、[AWS](http://d.hatena.ne.jp/keyword/AWS)のときはSQSを使ったりできるようなプラガブルな感じになっていると最高だ。そんなものを求めていくつか探してみた。

[github.com](https://github.com/dry-rb/dry-events)

-   みんな大好きdry-rbシリーズのPub/Sub[インターフェイス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%BF%A1%BC%A5%D5%A5%A7%A5%A4%A5%B9)
-   おそらく今回紹介するgemの中で一番シンプル。
-   `Dry::Events::Publisher`というモジュールをミクスインしたクラスを作ってサブスクライバを定義すると、そのクラスに対してイベントをパブリッシュできる。これだけ。
-   開発が比較的活発なのもいい

[gitlab.com](https://gitlab.com/kris.leech/wisper_next)

-   元々は[wisper](https://github.com/krisleech/wisper)というPub/Sub[インターフェイス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%BF%A1%BC%A5%D5%A5%A7%A5%A4%A5%B9)gemを作っていた作者による新しいライブラリ
-   `WisperNext.publisher`と`Wisper.subscriber`というふたつのモジュールが用意されておりそれらをミクスインしたクラスを作るスタイル
-   パブリッシャクラスに対してsubscribeメソッドでサブスクライバクラスを追加していく。
-   特定のプレフィクスを持つイベントのみをサブスクライブできる機能もある。
-   サブスクライブをする際に同期／非同期を選択できる。

[gitlab.com](https://gitlab.com/kris.leech/ma)

-   上と同じ作者のPub/Sub[インターフェイス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%BF%A1%BC%A5%D5%A5%A7%A5%A4%A5%B9)gem。内部的には`wisper_next`が使われている。
-   ライブラリの説明に "events as first class citizens." と書かれている通り、wisper\_nextとの違いはパブリッシュ／サブスクライブするイベントをクラスとして定義できること。`Ma::Event`クラスを継承したクラスをイベントとして扱うことができる。
-   それ以外には`Ma.publisher`と`Ma.subscriber`というモジュールが用意されており、それぞれをミクスインしたクラスを作るスタイルで特に目新しいものはない。

[github.com](https://github.com/edisonywh/rocketman)

-   今回紹介するgemの中で一番重厚かつ拡張性が高い。
-   `Rocketman::Prodcuer`と`Rocketman::Consumer`のふたつのモジュールがいて、それぞれをミクスインしたクラスで`emit`とか`on_event`みたいなメソッドが使える。まあわかりやすい。
-   Redis上のイベントもサブスクライブできるアダプタが用意されている。現状はRedisだけだが、Registryという仕組みが用意されているので自前でカスタムの外部サービス用アダプタを作ることも可能。
-   初期設定では発行されるイベントがキューとしてメモリ上に保存され、Rocketmanのスレッドワーカーが処理していく仕組みになっている。これだとアプリケーションが死んだときキューをロストするので、もしキューを永続化したければRedisをストレージとして使うことができる。
-   作者によるとRedis PubSubやKafkaはそのあたりの[ミドルウェア](http://d.hatena.ne.jp/keyword/%A5%DF%A5%C9%A5%EB%A5%A6%A5%A7%A5%A2)をメンテしたりするコストがかかるが、Rocketmanを使えば実質アプリケーションレイヤで同じことができるので、すごく楽だよ、というのが売りらしい。

アプリケーション層でPub/Subしたいだけならどれでもよさそう。もっとリッチに、イベントを永続化したり複数台構成で負荷分散とかに使いたい場合はrocketmanがよさそう。[GCP](http://d.hatena.ne.jp/keyword/GCP)とか[AWS](http://d.hatena.ne.jp/keyword/AWS)あたりのPub/Sub製品のアダプタも自前で作れそうだし。ただメンテされているのかどうかが若干気になるが...

[![Reactive Design Patterns](https://images-fe.ssl-images-amazon.com/images/I/51NRNEVroQL._SL160_.jpg "Reactive Design Patterns")](https://www.amazon.co.jp/exec/obidos/ASIN/1617291803/izumisy-22/)