---
category: "[[Clippings]]"
author: "[[RAKUS Developers Blog | ラクス エンジニアブログ]]"
title: サービス分割に備えたモノリス（モジュラーモノリスとかアグリゲートとか） - RAKUS Developers Blog | ラクス エンジニアブログ
source: https://tech-blog.rakus.co.jp/entry/20201026/microservice
clipped: 2023-09-12
published: 2020-10-26
topics: 
tags:
  - clippings
  - ラクス
  - エンジニア
  - 開発
  - 技術ブログ
  - エンジニアブログ
  - 開発者ブログ
  - ruby
  - マイクロサービス
  - モノリス
  - モノリシックアーキテクチャ
  - マイクロサービスアーキテクチャ
  - モジュラーモノリス
---

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/moomoo-ya/20201022/20201022123603.jpg)

こんにちは、株式会社[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)スで先行技術検証や、ビジネス部門に技術情報を提供する取り組みを行っている技術推進課に所属している鈴木（[@moomooya](https://twitter.com/moomooya)）です。

[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)スの開発部ではこれまで社内で利用していなかった技術要素を自社の開発に適合するか検証し、ビジネス要求に対して迅速に応えられるようにそなえる 「開（か）発の未（み）来に先（せん）手をうつプロジェクト（通称：かみせんプロジェクト）」 改め **「技術推進プロジェクト」** というプロジェクトがあります。

2020年度上期に「サービス分割を見越した[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)層設計」について取り組んだので、概要を紹介したいと思います。

今までの記事はかみせんカテゴリからどうぞ。 [tech-blog.rakus.co.jp](https://tech-blog.rakus.co.jp/archive/category/%E3%81%8B%E3%81%BF%E3%81%9B%E3%82%93)

---

## 今回の目標

2017年に社内で[マイクロサービスアーキテクチャについて検証した](https://tech-blog.rakus.co.jp/entry/20180926/microservice)際には「サービス立ち上げ時にはマイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)は選択するべきではない」という結果が導かれました。

この判断結果は2020年時点でも変わらないものだと感じています。とはいえサービスが大きくなったときに「開発速度を維持するためサービスの分割を検討することになる」というのも正だと思います[1](#fn:1)。

このとき必要に応じて「サービスを分割することが出来る」設計になっていなければ、年単位で移行したという事例が散見されるように、[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)からマイクロサービスへの移行は極めて難しい判断になるでしょう。

サービス分割の肝となる部分は「[ビジネスロジック](http://d.hatena.ne.jp/keyword/%A5%D3%A5%B8%A5%CD%A5%B9%A5%ED%A5%B8%A5%C3%A5%AF)＝[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)層が分割可能かどうか」という点だと考えています。なので今回のテーマは「サービス分割を見越した[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)層設計」と設定し調査検証を行っていきました。

### 余談：「開発速度を維持するために」サービス分割を検討するというのはなぜ

[プログラマー](http://d.hatena.ne.jp/keyword/%A5%D7%A5%ED%A5%B0%A5%E9%A5%DE%A1%BC)が扱える適切な[ソースコード](http://d.hatena.ne.jp/keyword/%A5%BD%A1%BC%A5%B9%A5%B3%A1%BC%A5%C9)量（プログラムの規模）には上限があります。[プログラミング言語](http://d.hatena.ne.jp/keyword/%A5%D7%A5%ED%A5%B0%A5%E9%A5%DF%A5%F3%A5%B0%B8%C0%B8%EC)等にも大きく依存しますが、1万行であるとか、1画面に収まる量であるとか、いろいろな観点で語られますが人間に認知限界が存在する限りどこかに限界があります。

そして（特に[エンタープライズ](http://d.hatena.ne.jp/keyword/%A5%A8%A5%F3%A5%BF%A1%BC%A5%D7%A5%E9%A5%A4%A5%BA)領域の）プログラムの[ソースコード](http://d.hatena.ne.jp/keyword/%A5%BD%A1%BC%A5%B9%A5%B3%A1%BC%A5%C9)は数十万をゆうに超え、数百万を超える場合もあります。明らかに人間が扱える上限を超えています。

上限を超えたプログラムがどうなるか、というと「ロジックのミス」「想定しないバグ」「修正漏れ」が増加することは想像に難くないと思います。そのため適切なボリュームになるようプログラムを分割する必要があると考えます。

## なぜ最初からサービス分割をしないのか

まずは「サービス立ち上げ時にはマイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)を選択しない」のはなぜなのかという話を振り返ります。マイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)を採用する、ということがどういうことなのか噛み砕くと以下のようになると思います。

-   複数の[Webサービス](http://d.hatena.ne.jp/keyword/Web%A5%B5%A1%BC%A5%D3%A5%B9)を開発・運用し、連携させる
-   （理想としては）[Webサービス](http://d.hatena.ne.jp/keyword/Web%A5%B5%A1%BC%A5%D3%A5%B9)ごとにチームを構成し、互いに内部の構造は依存しない

複数の[Webサービス](http://d.hatena.ne.jp/keyword/Web%A5%B5%A1%BC%A5%D3%A5%B9)として作ることでマイクロサービスのメリットとされる技術異質性が確保できたり、物理的にロジックの汚染を防ぐことは出来ますが、それらのメリットを覆すだけの運用コストが発生してしまいます。[Kubernetes](http://d.hatena.ne.jp/keyword/Kubernetes)などのコンテナ[オーケストレーション](http://d.hatena.ne.jp/keyword/%A5%AA%A1%BC%A5%B1%A5%B9%A5%C8%A5%EC%A1%BC%A5%B7%A5%E7%A5%F3)ツールを高レベルで使いこなすだけの運用スキルがチーム内にあれば解決できるかもしれませんが、結局「高レベルのスキル＝高コスト」なので立ち上げ時には向きません。

また[Webサービス](http://d.hatena.ne.jp/keyword/Web%A5%B5%A1%BC%A5%D3%A5%B9)ごとにチームを構成することで迅速な開発を行っていけるのもメリットですが、物理的に別サービスになっていることと、チームが分かれていることでサービス境界の定義の見直しがしにくくなります。サービス境界の定義が不適切だとマイクロサービス化して得られるはずのサービス感の独立性が失われるため、いわゆる「分散[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)」という[アンチパターン](http://d.hatena.ne.jp/keyword/%A5%A2%A5%F3%A5%C1%A5%D1%A5%BF%A1%BC%A5%F3)に陥ってしまいます。サービス立ち上げ時は一般的に[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)知識も乏しく適切なサービス境界の定義は難しいため選択しにくい選択肢になります。

またサービス立ち上げ時は「扱えない規模のプログラム規模拡大」も「大きな負荷」もまだ発生していません。利益が発生していない時点で、発生していない問題にコストを投入するのは悪手と言えるでしょう。

## [モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)で困ること

マイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)が流行し始めた2016年頃から「モノリシック[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)は悪」という主張が散見されました。これには誤りが含まれています。

まずモノリシック（1枚岩）[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)の特徴としてデプロイメントラインが1つであることが挙げられます。これは明確にマイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)に対してメリットです。複数のデプロイメントラインが存在するとその分デプロイコストはかさみます。

ではモノリシック[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)は何が「悪」なのか、というと体験している人も多いと思いますが、モジュール間の参照が複雑に交差することによるスパゲッティ化（[Big ball of mud](http://www.laputan.org/mud/) = 大きな泥団子、とも）が起こりやすいことがモノリシック[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)の真の問題です。

マイクロサービス[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)は構造的にスパゲッティ化が起こりにくい[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)です。しかし立ち上げ時にはコスト的に見合いません。であれば、スパゲッティ化しにくい方法（適切なモジュール分割）を適用した[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)が実現できれば立ち上げ時の有力な候補となりえると考えました。

## モジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)とは

適切なモジュール分割を行った[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)を調べていくとそれが「モジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)」と呼ばれていることを知りました。端的に言うと理想的なモジュール分割が行われた「きれいな[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)」です。

1つのプログラム内で独立性の高いモジュールとして分割されることで、規模が大きくなったときのサービス分割もやりやすそうに見えました。

またモジュール分割の境界線（サービス境界）を見直したくなった場合でも1つのプログラムの中の話なので、マイクロサービスに比べて見直しやすいとも感じます。

当然[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)なのでデプロイメントラインも1つです。

## [ビジネスロジック](http://d.hatena.ne.jp/keyword/%A5%D3%A5%B8%A5%CD%A5%B9%A5%ED%A5%B8%A5%C3%A5%AF)の設計方法

実際の[ビジネスロジック](http://d.hatena.ne.jp/keyword/%A5%D3%A5%B8%A5%CD%A5%B9%A5%ED%A5%B8%A5%C3%A5%AF)層の設計はDDDで出てくるアグリゲートパターン[2](#fn:2)が適用できそうです。 加えてファクトリパターン[3](#fn:3)も適用候補として準備しておくと良いと思います。ちなみに今回の検証ではファクトリパターンを候補にしていなかったため実装の段階で課題に直面することになりました。

詳細はDDD本を参照していただきたいですが、アグリゲートパターンは

-   アグリゲート（集約）
    -   関連するオブジェクトの集まり
    -   データを変更する単位（≒DB[トランザクション](http://d.hatena.ne.jp/keyword/%A5%C8%A5%E9%A5%F3%A5%B6%A5%AF%A5%B7%A5%E7%A5%F3)の単位）
-   アグリゲーションルート
    -   外部のアグリゲートとコミュニケーション可能な唯一のオブジェクト（＝エンティティ）

という概念に基づいていて、アグリゲートの生成もアグリゲーションルートのインタフェース（コンスト[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)タとか）により行います。

ただし、アグリゲートがある程度複雑になってくると生成ロジックも分離したほうがよくなってきます。生成ロジックの分離と[カプセル化](http://d.hatena.ne.jp/keyword/%A5%AB%A5%D7%A5%BB%A5%EB%B2%BD)を行ったものをファクトリとして扱うのがファクトリパターンです。 ファクトリパターンと言っても実装方法（実装場所）はさまざまなようです。

-   アグリゲートルート
    -   集約ルート内にファクトリメソッドとして実装
        -   もちろんコンスト[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)タでもよい
    -   アグリゲートパターンの部分で触れたケース
-   （アグリゲート内の）別のオブジェクト
    -   生成に関連がつよいオブジェクトにファクトリメソッドをもたせる
-   （アグリゲート外の）別のオブジェクト（もしくは別のサービス）
    -   生成ロジック的にアグリゲート内に収めるべきではない場合はアグリゲートの外におくことをためらわない

また、アグリゲートパターンに代わる別の設計手法としてMichael Nygard氏により「ライフサイクルによる分割([Services by Lifecycle](https://www.michaelnygard.com/blog/2018/01/services-by-lifecycle/))」も提唱されていますが、今回は未検証です。

## 実装と機能修正の範囲限定

結局実装してみないとわからないので試しに実装してみました。お題は弊社「[楽楽明細](https://www.rakurakumeisai.jp/)」のような帳票発行サービスをイメージして作ってみました。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/moomoo-ya/20201021/20201021222552.png)

アプリ（サービスレイヤ）の部分は[ウェブアプリケーション](http://d.hatena.ne.jp/keyword/%A5%A6%A5%A7%A5%D6%A5%A2%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3)[フレームワーク](http://d.hatena.ne.jp/keyword/%A5%D5%A5%EC%A1%BC%A5%E0%A5%EF%A1%BC%A5%AF)のルーティング処理の実装箇所だと思ってください。 上記のような設計をして、実際に実装してみたのですが先述の通りファクトリパターンを把握していなかったため「アプリ（サービスレイヤ）」の部分に初期化処理を実装してしまいました。 それの何が悪かったかというと機能追加のアップデートを実装したときに顕在化しました。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/moomoo-ya/20201021/20201021222556.png)

請求書のテンプレートを送付方法に応じて使い分ける事ができる機能を追加しました。送付方法は「メールにPDF添付」「印刷して郵送」を想定しています。点線部分は追加、修正が入ったモジュールになります。

見ての通り「アプリ（サービスレイヤ）」に修正が入ってしまってます。今回の前提（モジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)）であれば実質的な問題はほとんどありませんが、サービス分割後を想定するとサービス全体が停止することになってしまいます。ファクトリメソッドとしてアグリゲートルート内もしくはアグリゲート内にファクトリオブジェクトとして持っていれば「アプリ（サービスレイヤ）」には影響がなく、修正が入っていない受領者アグリゲート、売上レコードアグリゲートに関する機能は動かし続けることが可能になります。

### データストアについて

今回は利用していませんでしたが、データストア、主に[RDB](http://d.hatena.ne.jp/keyword/RDB)についても分けておいたほうが良いです。

物理的にDBサーバーを別で用意するとやはり手間がかかるので[MySQL](http://d.hatena.ne.jp/keyword/MySQL)でいうdatabase（[PostgreSQL](http://d.hatena.ne.jp/keyword/PostgreSQL)だとschema）の単位で分けるとか、もっと簡易にテーブル名で分けておくとかやり方はいろいろあると思いますが、分けておくと良いでしょう。

DBの分け方については参考文献の"Monolith to Microservices"のchapter.4が参考になると思います。

## 結論

実際に設計、実装してみた感じでもモジュール化（モジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)）を目指すことでサービス分割を低コストで実現することは可能そうに思えました。ただし、昔ながらの[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)より手間が増えることは間違いありません。手間が増える量がマイクロサービスに比べてはるかに少なく現実的な選択肢になるところまで落ち着く可能性がある、というのが適切な評価だと思います。

なので結局はバランスの問題ですが、ある程度サービス拡大が見込める場合には先行投資的にモジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)で構成し、賭けの要素が強いサービスの場合は[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)でコスト最小で昔ながらの[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)で構成する、といった選択になるのかな、と感じました。

### 今後の課題

アグリゲートパターンはやはりサービス境界（アグリゲート[バウンダリ](http://d.hatena.ne.jp/keyword/%A5%D0%A5%A6%A5%F3%A5%C0%A5%EA)、アグリゲート境界）の定義に[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)知識を必要とするため難しい、というのは解決されていません。[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)であることで取り返しはつくようになるものの[ドメイン](http://d.hatena.ne.jp/keyword/%A5%C9%A5%E1%A5%A4%A5%F3)知識が乏しい立ち上がり時に設計することは難易度が高いと思います。

「ライフサイクルによる分割([Services by Lifecycle](https://www.michaelnygard.com/blog/2018/01/services-by-lifecycle/))」ではサービスの利用フェイズをもとにサービス境界を定義していくという発想によりサービス境界の定義を比較的容易に行うことが出来る可能性を感じるため、ライフサイクルによる分割について今後検証していきたいと考えています。

## 参考文献

### 書籍

-   クリス・リチャードソン『[マイクロサービスパターン](https://amzn.to/3dvcP0R)』（[インプレス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%D7%A5%EC%A5%B9), 2020）
-   ヴァーン・ヴァーノン『[実践ドメイン駆動設計](https://amzn.to/3nCsbFr)』（[翔泳社](http://d.hatena.ne.jp/keyword/%E6%C6%B1%CB%BC%D2), 2016）
-   エリック・[エヴァ](http://d.hatena.ne.jp/keyword/%A5%A8%A5%F4%A5%A1)ンス『[エリック・エヴァンスのドメイン駆動設計](https://amzn.to/3obwPKK)』（[翔泳社](http://d.hatena.ne.jp/keyword/%E6%C6%B1%CB%BC%D2), 2011）
-   サム・ニューマン『[Monolith to Microservices](https://amzn.to/3dluwQ1)』（[オライリー](http://d.hatena.ne.jp/keyword/%A5%AA%A5%E9%A5%A4%A5%EA%A1%BC), 2019）
    -   洋書
    -   nginxから[無料版PDFが配布](https://www.nginx.com/resources/library/monolith-to-microservices/)されています

### 論文

-   Brian Foote & Joseph Yoder, "[Big Ball of Mud](http://www.laputan.org/mud/)"

### ブログ記事

-   [The awesomeness of Modular Monolith](https://no-kill-switch.ghost.io/the-awesomeness-of-modular-monolith/) (投稿日: 2017/05/08)
    -   Sebastian Gebski
        -   CTO at [Gemius](https://www.gemius.com/)（デジタル[マーケティング](http://d.hatena.ne.jp/keyword/%A5%DE%A1%BC%A5%B1%A5%C6%A5%A3%A5%F3%A5%B0)企業）
    -   マイクロサービスへのアンチテーゼ
-   [The Modular Monolith: Rails Architecture](https://medium.com/@dan_manges/the-modular-monolith-rails-architecture-fb1023826fc4) (投稿日: 2018/01/23)
    -   Dan Manges
        -   CTO at [Root Insurance](https://www.joinroot.com/)（[自動車保険](http://d.hatena.ne.jp/keyword/%BC%AB%C6%B0%BC%D6%CA%DD%B8%B1)）
        -   元創業CTO at [Braintree](https://www.braintreepayments.com/)（支払いプラットフォーム）
    -   [Rails](http://d.hatena.ne.jp/keyword/Rails)アプリの[アーキテクチャ](http://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3)としての提案
    -   今回扱う内容についての言及はおそらくこれが最初
-   [Deconstructing the Monolith: Designing Software that Maximizes Developer Productivity](https://engineering.shopify.com/blogs/engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity) (投稿日: 2019/02/21)
    -   Kirsten Westeinde
        -   Dev. Mgr. at Shopify
    -   Shopifyによる事例紹介
-   [Modular Monoliths — A Gateway to Microservices](https://medium.com/design-and-tech-co/modular-monoliths-a-gateway-to-microservices-946f2cbdf382) (投稿日: 2019/03/31)
    -   Natalie Conklin
-   [Modular Monolith Primer](https://www.kamilgrzybek.com/design/modular-monolith-primer/) (投稿日: 2019/12/03)
    -   Kamil Grzybek
        -   engineer at [ITSG Global](https://itsg-global.com/)
    -   モジュラー[モノリス](http://d.hatena.ne.jp/keyword/%A5%E2%A5%CE%A5%EA%A5%B9)入門
-   [モノリスの分解において、マイクロサービスは必然ではない - QCon LondonにおけるSam Newman氏の講演より](https://www.infoq.com/jp/news/2020/06/monolith-decomposition-newman/) (投稿日: 2020/06/29)

---

-   **エンジニア[中途採用](http://d.hatena.ne.jp/keyword/%C3%E6%C5%D3%BA%CE%CD%D1)サイト**  
    [ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)スでは、エンジニア・デザイナーの[中途採用](http://d.hatena.ne.jp/keyword/%C3%E6%C5%D3%BA%CE%CD%D1)を積極的に行っております！  
    ご興味ありましたら是非ご確認をお願いします。  
    [![20210916153018](https://cdn-ak.f.st-hatena.com/images/fotolife/t/tech-rakus/20210916/20210916153018.png)](https://career-recruit.rakus.co.jp/career_engineer/?utm_source=techblog&utm_medium=lp&utm_campaign=recruit&utm_content=footer_lp)  
    [https://career-recruit.rakus.co.jp/career\_engineer/](https://career-recruit.rakus.co.jp/career_engineer/)
    
-   **カジュアル面談お申込みフォーム**  
    どの職種に応募すれば良いかわからないという方は、カジュアル面談も随時行っております。  
    以下フォームよりお申込みください。  
    [rakus.hubspotpagebuilder.com](https://rakus.hubspotpagebuilder.com/visit_engineer/)
    
-   **[ラク](http://d.hatena.ne.jp/keyword/%A5%E9%A5%AF)スDevelopers登録フォーム**  
    [![20220701175429](https://cdn-ak.f.st-hatena.com/images/fotolife/t/tech-rakus/20220701/20220701175429.png)](https://career-recruit.rakus.co.jp/career_engineer/form_rakusdev/?utm_source=techblog&utm_medium=rd_lp&utm_campaign=rd_lp&utm_content=rd_lp)  
    [https://career-recruit.rakus.co.jp/career\_engineer/form\_rakusdev/](https://career-recruit.rakus.co.jp/career_engineer/form_rakusdev/)
    
-   **イベント情報**  
    会社の雰囲気を知りたい方は、毎週開催しているイベントにご参加ください！
    

**◆TECH PLAY**  
[techplay.jp](https://techplay.jp/community/rakus)

**◆connpass**  
[rakus.connpass.com](https://rakus.connpass.com/)