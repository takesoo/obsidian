---
finished reading: true
favorite: true
tags:
  - clippings
---
# プロダクトの課題をデータ分析から解決するための Step by Step｜piqcy
  #ReadItLater 
 #ReadableArticle

## articleURL
https://note.com/piqcy/n/n2d5e5c71a08e

## siteName
note（ノート）

## date
2025-05-22

## articleContent
データ分析の方法は知っているものの、分析してどうするか具体的なイメージが沸かない方は少なからずいるのではないでしょうか。具体的なサービスの例を使って経営課題の提示からデータ分析に基づく解決案の策定までをきれいに語った動画を見つけたので、ご紹介します。"[Webinar: Product Discovery With Data & User Research by Glovo Group PM, Lokesh Mahajan](https://youtu.be/tuo3CCfCHao)" という動画で、語っているのはスペインや中東・アフリカで配達サービスを提供している [Glovo](https://glovoapp.com/) という会社の Group Product Manager である [Lokesh Mahajan](https://www.linkedin.com/in/lokesh-mahajan-6807543a/) さんです。Oracle で IT コンサルタント、 Meta でプロダクト分析に関わり同種のサービスである [Siggy](https://www.swiggy.com/) のプロダクトマネージャーを担当した後 Globo で活躍されています。

本記事は動画のステップに沿って補足の説明を入れながら解説していきます。

トップ画像は [Tony Alter](https://www.flickr.com/photos/78428166@N00/) さんの[写真](https://flic.kr/p/6Vnjr5)を引用しました。

## Step0. プロダクトの理解

動画の中で題材として扱われるのは **Gymmie** という月額課金で様々なジムを利用できるサービスです。料金は €79 / 月 ( 約 12,000 円 / 月 ) で、 Gymmyland という国の 5 つの大都市で展開しており競合は現時点ではありません。

アプリを利用しジムにチェックインする方式をとっており、 ユーザーは 100 を超えるブランドの 7,500 店舗を利用することができます。月に 5 万人のアクティブユーザーが 60 万回チェックインするトラフィックがあります (= 一人当たり 12 回/月利用  )。

## Step1. 経営課題の理解

Gymmie の CEO は、より多くの国に展開し B2B のビジネスも立ち上げたいと考えています。実現するには営業、広告に大きな投資をする必要がありますが、資金集めに苦労しています。そこで、既存のサービスから資金を捻出したいと考えています。もちろん、新規ユーザーを獲得するための広告費は出せない前提です。

個人的に、 CEO の課題、つまり経営課題からスタートしているのは何気に重要なポイントと感じました。というのも、データを集めて分析しているのに「答えるべきビジネス上の質問がない」というのはデータ活用の失敗事例としてよくあることのためです ( 詳細は下記記事ご参照ください ) 。

[DX 白書 2023](https://www.ipa.go.jp/publish/wp-dx/dx-2023.html) によれば米国では 40.7% が設置しているデータ分析活用の上級管理職 (CDO) の設置が日本では 7.8% に留まっています。

そのため海外でも、日本ではさらに "Step1. 経営課題の理解" から始めることが重要と言えます。

## Step2. データの理解

次に、課題に直結するデータを分解していくことで理解を深めていきます。今回の場合はもちろん利益です。利益は売上からコストを差し引くことで得られます。月次の売上はアクティブユーザー数と月額課金額の積、コストはアクティブユーザー数・平均チェックイン数・チェックイン手数料の積から計算できます。 Gymmie では、ジム会社と事前に取り決めたチェックイン手数料をチェックイン数に応じて支払う形式をとっています。

売上を増やすには MAU を上げるか課金額を上げればよいことになります。ただ、前提条件より広告による獲得は困難で、かといってサービス品質を変えずに課金額を上げることはユーザー体験の棄損に直結し好ましいアクションではありません。

コストを減らすにはチェックインの数か手数料を下げるのが有力です。ただ、チェックインの数が下がればサービスの利用頻度自体が下がってしまいますし、ジムに支払う手数料を下げるようお願いすると加盟してくれるジムが減ってしまいユーザー体験の棄損に直結します。

基礎的な分解から考えられる解決策は手詰まりなので、さらに"平均"となっている手数料とチェックイン数についてより分解を進めます。

"平均"の手数料は €5.25 ですが、多くのジムは €3.00 で一部のジムに高い手数料を払っていることがわかります。

そして、手数料が高いジムへのチェックインが多いことがわかります。

ブランドや交渉力のある「大手のジム」の手数料が高く、さらにユーザーは「大手のジム」をより好む傾向があると言えます。この傾向は Gymmie にとって好ましい傾向と言えるでしょうか ? ( いえるのであれば、是正する必要がないことになります ) 。少数の大手ジムへの依存は、高い手数料だけでなく大手のジムが契約を停止した時にユーザーが離脱するリスクを高めることになり、経営上好ましいとは言えません。

個人的に、 Step2 で判明した傾向を Step1 、経営の視点で見返す点は重要と感じました。データ分析のプロセス定義として [CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining) が著名ですが、ここでもビジネス理解とデータ理解は相互に行き来するようになっています。

大手ジムに依存している => 依存を下げよう、と原因と対策を裏表にして施策を打つことをやってしまいがちですが、経営から見て適切な状況であればひっくり返すことは好ましくありません。

## Step3. 問題の定義

データの傾向から解決すべき問題をどのように定義できるでしょうか。定義の仕方はいくつかありますが、動画ではデータより 82% のユーザーのチェックインが同じジム、とくに大手のジムからきていることが問題、と定義しています。

この傾向はチェックイン数が少ないユーザーで顕著です。ユーザーの獲得とリテンションを大手ジムに依存している点がより問題と言えます。

さきほど、[CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining) ではビジネス理解とデータの理解を行き来すると述べましたが、問題の定義か完了するタイミングが行き来の終了と言えると思います。定義された問題は、 1) 経営課題と直結し、2) 数字で表されている必要があります。

## Step4. 定量・定性データの収集

問題を解決するため、原因を示唆する定量・定性データを収集します。今までの分析で問題の特定はできましたが、ユーザーが大手ジムを利用する理由は明らかになっていません。原因を特定するには、ユーザーの行動についてデータを収集する必要があります。

ユーザーの動線を定量的に分析してみましょう。 Gymmie では 3 つジムを探す動線があります。 Gym Listing はジムのリストページで 55% がジムの Discovery ( ジムのページへ遷移する確率 ? ) に繋がり 74% は大手ジムへの予約 (Top Brands Reservations) になっています。 Activity Discovery は発見につながる率は 25% と低いものの Top Brands Reservations は 31% で大手以外のジムの予約に繋がりやすくなっています。( 検索の必要がない ) 大手以外のジムを発見/予約するために使うと思われる Search と同等の Top Brands Reservations で、大手以外のジムを見つけるのに有効な動線といえます。

ユーザーが大手のジムを選ぶ定性的な理由をヒアリングしてみましょう。良く知らないジムに行くのは怖い、複数人で行くので人気があるところに決まりやすい、同じジムに行くことがルーティンになりやすい、マーケティングのためよく知られている、などのフィードバックが得られました。

定量・定性データを得るには様々な手法があります。プロダクトが成熟しているほどとれる手段のバリエーションが多くなります。下図に手法の分類を示しますが、詳細が気になる方は「[データ活用事例が自分のプロダクトにフィットしないのはなぜなのか](https://note.com/piqcy/n/n03e88f666fa7)」を参照してください。

## Step5. 問題の解決を示す KPI を設定する

どのような KPI が定義した問題の解決を示すでしょうか ? よい KPI はビジネスの問題とユーザーの問題両方の解決に繋がります。「大手ジムへのチェックイン数」はよい KPI とは言えません。ユーザーにとって大手ジムの手数料が高いことは関係なく、理由があって大手ジムを選んでいます。大手ジムのチェックイン数を減らすことはビジネスの問題を解決してもユーザーの問題を解決しない、というか逆に問題を生むことになるでしょう。根本的な問題はユーザーが大手ジム以外を試さないことであり、チェックイン数の多いユーザーが大手以外を選んでいることから、より他のジムを試すことで大手以外の適切なジムの利用につながると考えられます。

いくつか KPI の案はありますが、N 回のチェックインにおける試したジムの平均は候補になります。この KPI はビジネスの問題を解決し、ユーザーにとっても最終的に利用するジムに早くたどり着ける点でメリットがあります。

個人的に、最終的に機械学習での最適化を見据える場合「機械学習モデルの精度改善につながる」のも条件と思います。モデルの精度が高まればより顧客の体験を改善し、ビジネスの成長にもつながるからです。以下は [ML Enablement Workshop](https://github.com/aws-samples/aws-ml-enablement-workshop) の資料から引用した図です。

## Step6. 解決につながる仮説を立てる

解決策についての仮説を立てます。今回は、次の 3 つが候補となります。

1.  近くで行える様々なアクティビティを提案すれば、様々なジムの発見につながる。
    
2.  あまり知られていないブランドのジムの信頼性を増せば、様々なジムを試すことにつながる。
    
3.   ( メインの動線である ) Gym Listing の表示順を最適化すれば、大手ジムへの依存を減らせる。
    

Step6 の内容が非常にあっさりしていることからわかる通り、問題の定義が済み (Step3) 、データがあり (Step4) 、KPI の設定が済んでしまうと (Step5) 仮説を立てることにそれほど労力はかかりません。

## Step7. 解決策を検証する

仮説に基づく解決策を立て、検証します。例えば、 Gym Listing の中で Activity から ( 下図左 ) やトレイナーから ( 下図右 ) 見つける要素を取り入れることでより幅広なジムのお試しを促します。機械学習を使ったリスティングをしていれば、ランキングアルゴリズムを変えたり目的変数を変える手段もあるでしょう。

知られていないジムの信頼性を上げる手法として、レーティングやレビューの数値を表示して信頼性を示すことが上げられます。 Amazon's Choice のように一定のクライテリアを満たしたジムに認証マークを付けることなども考えられます。

解決策を「検証」するには、解決策を実行するだけでなく設定した KPI をモニタリングすることが不可欠です。解決策が上手く行かない場合は、 Step6/Step7 を繰り返すことで改善をします。

個人的にポイントと感じるのは、上記の解決策はいずれも「ユーザーの要望」から生まれないことです。ユーザーは大手ジムの利用に特段不満がないためです。逆に、大手ジムをもっと増やしてほしい / 表示してほしい、という声の方が多いかもしれません。ユーザーとしては自分の課題が解決すれば OK ですが、 CEO としてはあらゆる人の課題を解決したいわけで、課題解決をスケールするにはビジネスの成長が不可欠です。だからこそ、ビジネスと顧客体験がバランスした KPI に基づく解決策が重要と言えます。

## おわりに

動画の内容は以上です。記事中に資料を抜粋しましたが、 AWS が無料で資料を公開している ML Enablement Workshop は、プロダクトで機械学習を活用できるようになるためのワークショップです。ワークショップ本体の資料に加え、本記事のような先進的プロダクトで活躍する方のノウハウを[データサイエンスを活用するプロダクトマネージャーを訪ねて](https://github.com/aws-samples/aws-ml-enablement-workshop/blob/main/docs/journal/README.md) と題し Q&A 形式でまとめたコンテンツを掲載しています。もちろん、ワークショップの資料には随時この Q&A の内容が取り込まれています！ GitHub で Star をしていただくとすぐにアクセス出来て、また Watch すると更新通知が来て便利です。

AWS からのワークショップ提供に関心ある方はカジュアルトークをしていますのでぜひお声がけください。