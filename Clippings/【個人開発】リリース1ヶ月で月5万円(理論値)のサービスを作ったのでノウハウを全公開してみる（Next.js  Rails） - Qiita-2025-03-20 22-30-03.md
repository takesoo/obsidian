---
finished reading: true
favorite: false
tags:
  - clippings
---
# 【個人開発】リリース1ヶ月で月5万円(理論値)のサービスを作ったのでノウハウを全公開してみる（Next.js / Rails） - Qiita
  #ReadItLater 
 #ReadableArticle

## articleURL
https://qiita.com/tomada/items/b992245a4162ddeb1f6e

## siteName
Qiita

## date
2025-03-20

## articleContent
こんにちは、とまだです。

みなさん、個人開発はしていますか？

そして個人開発をしている方は **個人開発アプリで一発あてて月収 xx 万円** を夢見ていたりしませんか？

私も夢見る一人なのですが、特に「集客」や「マネタイズ」の方法に悩んでいる方も多いのではないでしょうか。

今回は、私が個人開発した **[Learning Next](https://learning-next.app/)** というサービスがあります。

[![スクリーンショット 2025-03-18 8.06.00.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6a1f0455-0d44-44fd-a251-7363c4f301b3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=51e3fb3d4957715d8746be42849b2953)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6a1f0455-0d44-44fd-a251-7363c4f301b3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=51e3fb3d4957715d8746be42849b2953)

こちらが軌道に乗ってきたので、開発の背景から技術的な工夫まで、赤裸々にお話ししたいと思います。

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%93%E3%81%AE%E8%A8%98%E4%BA%8B%E3%82%92%E8%AA%AD%E3%82%93%E3%81%A0%E3%82%89%E3%82%8F%E3%81%8B%E3%82%8B%E3%81%93%E3%81%A8)この記事を読んだらわかること

この記事は、個人でサービスを作ってみたい人、特に **集客やマネタイズの方法に悩んでいる人** に向けて、以下のような内容をお伝えします。

-   リリース 1 ヶ月での成果（理論値）
-   個人開発の Web アプリで集客を増やすための工夫
-   SEO 対策のポイント
-   マネタイズの考え方
-   ユーザー目線での UI/UX 改善
-   プロの Web エンジニアが個人開発するときに意識していること

自分のためのメモという意味合いもありますが、他にも個人開発をしている方にとって参考になる情報があれば幸いです！

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E7%B0%A1%E5%8D%98%E3%81%AB%E8%87%AA%E5%B7%B1%E7%B4%B9%E4%BB%8B)簡単に自己紹介

フリーランスの Web エンジニアをやっています「とまだ」です。

開発においてはフロントエンドからバックエンド、インフラにクラウドまでフルスタックに何でもやっています。

あまり細かくは書きませんが、本業では月間 500 万 PV 以上のサイトのフロント・バックエンド開発や、SEO 対策など幅広く対応しています。

[![スクリーンショット 2025-03-18 8.50.17.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fbe947765-c0b5-42a2-b3fa-543c217ef1ba.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9fcbe7f6d398affcb2315ad1ca87a62d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fbe947765-c0b5-42a2-b3fa-543c217ef1ba.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9fcbe7f6d398affcb2315ad1ca87a62d)

また、ありがたいことに、RSpec x Rails の講座で Udemy 公式の **ベストセラー** をいただいており、プログラミング教育にも力を入れています。

-   [Udemy プロフィール](https://www.udemy.com/user/zeng-shan-you-si)

[![rspec_udemy_bestseller.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fd02e8dd4-b404-419d-af1c-cfbe45b7834b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=49d5e6b910687367beeb04107ecd29fd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fd02e8dd4-b404-419d-af1c-cfbe45b7834b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=49d5e6b910687367beeb04107ecd29fd)

そんな私が現場や個人で培ってきたノウハウを詰め込みまくったサービスについて、「考え方」や「ノウハウ」の観点でご紹介します！

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9-1-%E3%83%B6%E6%9C%88%E3%81%A7%E3%81%AE%E6%88%90%E6%9E%9C%E7%90%86%E8%AB%96%E5%80%A4)リリース 1 ヶ月での成果（理論値）

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%BE%E3%81%9A%E3%81%AF%E3%83%9E%E3%83%8D%E3%82%BF%E3%82%A4%E3%82%BA%E3%81%AE%E6%88%90%E6%9E%9C%E3%81%8B%E3%82%89)まずはマネタイズの成果から

まずは、マネタイズの成果からお伝えしましょう。

1 ヶ月での収益は、**5 万円/月** （理論値）です。

「理論値」という言い方から分かる通り、実際にはこの金額を得ることはできていません（大げさ太郎でごめんなさい）。

2025 年 2 月に正式リリースしてマネタイズを開始してから、最高の 1 日報酬が **1,620 円** でしたので、31 日で約 **5 万円** という計算です。

[![スクリーンショット 2025-03-10 18.19.07.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6011a43f-4ea7-4df2-8196-383b1927dad3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fc465b8e07d345de1d017d2d9ad157a6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6011a43f-4ea7-4df2-8196-383b1927dad3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fc465b8e07d345de1d017d2d9ad157a6)

ただ、最高瞬間風速とはいえ「ドメイン評価は激弱」「個別の宣伝なし」の中、Google 検索流入だけで成果が出たのは、自分でも驚きです。

後述しますが、**SEO 対策** といった工夫を重ねた結果、検索エンジンからの流入が増え、それが収益につながったと考えられます。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%A9%E3%81%86%E3%82%84%E3%81%A3%E3%81%A6%E5%8F%8E%E7%9B%8A%E3%82%92%E5%BE%97%E3%81%A6%E3%81%84%E3%82%8B%E3%81%AE%E3%81%8B)どうやって収益を得ているのか？

Learning Next では「Udemy 講座のアフィリエイト」、つまり紹介した講座が購入された際に報酬を得る仕組みを導入しています。

サービスとしては [LinkShare](https://www.linkshare.ne.jp/) を利用しており、購入数や金額に応じて報酬が発生します。

[![スクリーンショット 2025-03-18 8.04.58.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fad9608de-9ff6-4ae4-9d53-b6c5957a5b7e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f1b73ced02d616faebbed210635bf27f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fad9608de-9ff6-4ae4-9d53-b6c5957a5b7e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f1b73ced02d616faebbed210635bf27f)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#udemy-%E3%82%A2%E3%83%95%E3%82%A3%E3%83%AA%E3%82%A8%E3%82%A4%E3%83%88%E3%82%92%E9%81%B8%E3%82%93%E3%81%A0%E7%90%86%E7%94%B1)Udemy アフィリエイトを選んだ理由

Udemy は、プログラミングをはじめとする幅広いジャンルの講座が揃っていることが強いところです。

さらに、何より自分が Udemy を使って学び続けてきた経験があるので、信頼性が高いと感じていました。

このあたりは [技術ブログ](https://musclecoding.com/category/programming-study/udemy/) の方でも使っているので、使い方を含め自信があったため、マネタイズの軸として選びました。

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%AA%E3%81%9C%E3%81%93%E3%81%AE%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%82%92%E4%BD%9C%E3%82%8D%E3%81%86%E3%81%A8%E6%80%9D%E3%81%A3%E3%81%9F%E3%81%AE%E3%81%8B)なぜこのサービスを作ろうと思ったのか

ではここで、一番大事な「モチベーション」についてお話しします。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E6%9C%AA%E7%B5%8C%E9%A8%93%E3%81%8B%E3%82%89%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%81%AB%E3%81%AA%E3%82%8C%E3%81%9F%E3%81%93%E3%81%A8%E3%81%8B%E3%82%89%E3%81%AE%E6%80%9D%E3%81%84)未経験からエンジニアになれたことからの思い

私自身、プログラミング学習の多くを Udemy に頼ってきました。

プログラミング未経験からエンジニアになる際や、今でも新しい技術を学ぶときには、Udemy の講座を利用することが多いです。

ただ、Amazon で買い物するときもそうですが、 **ハズレを引きたくはない** ので講座選びにはかなり慎重になりますよね。

Udemy には 5 点満点の評価や受講生数から「ざっくり」と良さはわかるものの、 **「自分に合った講座」を見つけるのは難しい** と感じていました。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F504734c5-ace3-4114-8fc9-59d40e580bd0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=267ddefdf1f6d5944b70fc44d741c299)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F504734c5-ace3-4114-8fc9-59d40e580bd0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=267ddefdf1f6d5944b70fc44d741c299)

たとえば、タイトルには「入門」と書いてあるのに、実際には少し学んでないと難しい内容だったりします。

また、講座の内容が古くなっていたり、講師の説明が分かりづらかったりすると、学習効率が落ちてしまいます。

しかし、特にプログラング初心者だと説明文からはそうしたポイントがわかりにくいですよね...。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E6%82%A9%E3%81%BF%E3%82%92%E8%A7%A3%E6%B1%BA%E3%81%99%E3%82%8B%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%8C%E3%81%82%E3%82%8C%E3%81%B0%E3%81%84%E3%81%84%E3%81%AA%E3%81%82)悩みを解決するサービスがあればいいなあ〜

そうした自分の悩みを解決するために、 **「講座選びの難しさ」** に着目しました。

表層的な「人気度」や「星評価」だけでなく、 **「実際に受講した人の声」** をもとに「講座の良し悪し」を客観的に評価するサービスを作ろうと思い立ちました。

レビューには、受講者の「生の声」が詰まっていますからね。

[![スクリーンショット 2025-03-18 8.16.45.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F8f0a4de4-c3df-4297-b1a6-9ac0d3141945.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ae13e9f151e1cb4cc288cb361b5a5f92)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F8f0a4de4-c3df-4297-b1a6-9ac0d3141945.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ae13e9f151e1cb4cc288cb361b5a5f92)

ただ、そのレビューをただ載せるだけではなく、 **AI を使って自動で分析・要約** することで、多くのレビューを一気に把握できるようにしました。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%AA%E3%81%9Cai-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E5%88%86%E6%9E%90%E3%81%AA%E3%81%AE%E3%81%8B)なぜ「AI を使ったレビュー分析」なのか

まずモチベーションとしては **AI でナウいことをやりたかった** というのがあります。

が、真面目に理由を付けるなら、以下のようなメリットがあるからです。

-   多くのレビューを一気に把握できる
-   自動化により、継続的な分析が可能
-   個人開発でも大量のデータを処理できる
-   **客観的な評価基準を設けられる**

特に、人気度や受講生数に左右されることなく、機械的にレビューを分析することで、 **客観的な評価基準** を設けられるのが大きなメリットです。

[![スクリーンショット 2025-03-18 8.30.01.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fae2cf418-558f-4caf-8573-8ec5b58f2a44.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4fdb2275dfecca0eac77e0afecbc5784)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fae2cf418-558f-4caf-8573-8ec5b58f2a44.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4fdb2275dfecca0eac77e0afecbc5784)

AI と言っても Claude の API を使っているだけはありますが、普段から自然言語処理に興味があったので、これを機に実際に使ってみたかったというのもあります。

また、副業でプロンプトエンジニアリングの教育業務もしているので、持っている知識を自分のアプリに活かしてみたかったというのもあります。

では、次に実際にどのようなサービスを作ったのか、その特徴や工夫についてお話しします！

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#learning-next-%E3%81%AE%E7%89%B9%E5%BE%B4)Learning Next の特徴

さて、ここからは **[Learning Next](https://learning-next.app/)** の特徴や工夫についてお話しします。

個人開発のノウハウというよりは、サービスとして **こだわったポイント** について紹介しますので、考え方の参考にしていただければと思います。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#ai-%E3%82%92%E6%B4%BB%E7%94%A8%E3%81%97%E3%81%9F%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E5%88%86%E6%9E%90)AI を活用したレビュー分析

**Learning Next** では、Udemy 上にある大量のレビューを **Claude** の API を使って自動で分析・分類しています。  
具体的には、各講座に対して投稿されている数十件以上ものレビューをまとめて投げかけ、ポジティブな面、ネガティブな面、そして「どういう人に向いているのか」を要約させる仕組みをつくりました。

-   例：[【Udemy レビュー】現役シリコンバレーエンジニアが教える Python 3 入門](https://learning-next.app/udemy/courses/python-beginner)

[![スクリーンショット 2025-03-11 14.08.01.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F2aec9b1c-f4ec-49d7-ba0b-df66ef06eeeb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4beefd8508a948f1c0a1ac7a2d6580c4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F2aec9b1c-f4ec-49d7-ba0b-df66ef06eeeb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4beefd8508a948f1c0a1ac7a2d6580c4)

これにより、「レビュー全文を読み込む手間」や「単なる星評価だけではわからないリアルな声」を、要点だけピックアップして把握できます。

特にネガティブ面は、学習者にとって重要な情報ですので、これを自動で抽出できるのは大きなメリットですね。

「音声が少し聞き取りにくい」「前提知識が想像以上に必要だった」など、受講して初めて気づくようなポイントを事前に知れるので、講座選びの参考になるかと思います。

[![スクリーンショット 2025-03-16 21.58.49.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F17936cc2-ced0-43ca-a48c-b74b7d789fd1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=55c3c9abdf628d26e45365fa503f4084)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F17936cc2-ced0-43ca-a48c-b74b7d789fd1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=55c3c9abdf628d26e45365fa503f4084)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#4-%E3%81%A4%E3%81%AE%E8%A9%95%E4%BE%A1%E8%BB%B8%E3%81%A7%E3%82%B9%E3%82%B3%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0)4 つの評価軸でスコアリング

レビュー分析の結果を、私は以下の 4 つの観点でスコア化しています。  
単に「総合評価」という一括りではなく、それぞれの講座の強み弱みを **数値として比較できるようにした** のがポイントです。

こちらは、[現役シリコンバレーエンジニアが教える Python 3 入門](https://learning-next.app/udemy/courses/python-beginner) のレビュー内容をもとに AI で分析した結果です。

[![スクリーンショット 2025-03-11 14.04.03.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F070048dd-21a7-4f19-80d1-f8797867e1c7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e8cf91e17603a55c8ac9528dbf86b261)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F070048dd-21a7-4f19-80d1-f8797867e1c7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e8cf91e17603a55c8ac9528dbf86b261)

-   **わかりやすさ（10 点満点）**
    -   初心者が理解しやすい構成かどうか
    -   講師の説明が丁寧か、コード例が理解しやすいか
-   **実践力（10 点満点）**
    -   実務に直結するようなプロジェクトがあるか
    -   応用力や最新トレンドに対応しているか
-   **サポート体制（10 点満点）**
    -   講師の質問対応は丁寧で早いか
    -   コミュニティや学習フォーラムが活発か
-   **教材品質（10 点満点）**
    -   動画や音声のクオリティ
    -   補助資料、ダウンロード可能なリソースの充実度

受講したい講座を選ぶとき、自分が最も重視する部分は人によって違いますよね。

「まったくの初心者なので、わかりやすさを最優先したい」「現場で使えるスキルをとにかく身につけたい」など、ニーズは多種多様かと思います。

こうしたニーズに合わせて評価項目を参考にできるようにしました。

（レーダーチャート実装したいなあ...）

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E6%9F%94%E8%BB%9F%E3%81%AA%E6%A4%9C%E7%B4%A2%E3%83%95%E3%82%A3%E3%83%AB%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0)柔軟な検索・フィルタリング

講座を探すときのキーワードや条件は、人によってさまざまですよね。  
そこで、**Learning Next** ではカテゴリやトピックを細かく設定し、絞り込み検索をサクサクできるようにしています。

[トピック別の Udemy 講座](https://learning-next.app/udemy/topics)

[![スクリーンショット 2025-03-18 8.31.54.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fbdd501df-2ab6-4e52-a0bd-6e5e2cf5c98d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f36e522b837d434b20c0434eff14f28e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fbdd501df-2ab6-4e52-a0bd-6e5e2cf5c98d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f36e522b837d434b20c0434eff14f28e)

[フィルタリング機能](https://learning-next.app/udemy/courses)

[![スクリーンショット 2025-03-18 8.36.44.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F1e24b07e-a67c-4717-96fe-c0a2d31aaba1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2978d7d9586aacb6095a49f5356c93da)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F1e24b07e-a67c-4717-96fe-c0a2d31aaba1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2978d7d9586aacb6095a49f5356c93da)

また、キーワード検索もできるので、特定の講座を探すときにも便利です。

[![スクリーンショット 2025-03-11 14.04.48.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F5295b5e8-0acb-4c36-abdd-df3d62bdb22f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=13ddc04ea88f640e35ca918ba024633b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F5295b5e8-0acb-4c36-abdd-df3d62bdb22f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=13ddc04ea88f640e35ca918ba024633b)

網羅的な情報を載せるサイトこそ、ユーザーが求める情報を素早く見つけられるようにすることが大事だと考えています。

よく、フィルタリングはできても「特定の講座が見つけにくい」という声を聞きますので、そのあたりにも配慮しています。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E5%AD%A6%E7%BF%92%E3%83%AD%E3%83%BC%E3%83%89%E3%83%9E%E3%83%83%E3%83%97%E3%81%AE%E6%8F%90%E4%BE%9B)学習ロードマップの提供

これは講座のレビュー分析とは直接関係はありませんが、プログラミング学習を始める人にとっては「次に何を学べばいいのか」がわかりにくいことがありますよね。

また、検索キーワードのボリュームを見ていると「React 学習 ロードマップ」といった検索が多いことがわかっています。

[![スクリーンショット 2025-03-18 8.33.06.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F1cfa36ae-d56c-4678-9d73-72f645143df2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bd8a18aec2c078b2cd278b97cc083771)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F1cfa36ae-d56c-4678-9d73-72f645143df2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bd8a18aec2c078b2cd278b97cc083771)

そのため、 **Learning Next** では「フロントエンドエンジニアになりたい」「バックエンドエンジニアになりたい」といった目標に合わせた学習ロードマップを提供しています。

-   [【目的・職種別】未経験からエンジニアを目指すためのおすすめ学習ロードマップ](https://learning-next.app/roadmap)

こちらは一例です。

こだわりとしては、一口に「バックエンドエンジニアになりたい」といっても言語やフレームワークは様々ですので、それに合わせたロードマップを提供しています。

[![スクリーンショット 2025-03-18 8.33.56.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fefdcdabb-d61e-48df-be29-62361c00414b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=eb64d9ad0dcbb0f778b3a511f5e5a732)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fefdcdabb-d61e-48df-be29-62361c00414b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=eb64d9ad0dcbb0f778b3a511f5e5a732)

僭越ながら、私はバックエンドやフロントエンド、インフラにクラウドと幅広い技術を使えるので、経験を活かしてロードマップを作成しました。

もちろん、自分の知識ではカバーし切れていない分野もあります。

そのあたりは技術・市場調査をしつつ、ChatGPT / Claude の力で一部修正して、できるだけ幅広い分野で情報を提供できるようにしています。

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%84%E3%81%8B%E3%81%AB%E3%81%97%E3%81%A6%E9%9B%86%E5%AE%A2%E3%81%99%E3%82%8B%E3%81%8Bseo-%E5%AF%BE%E7%AD%96%E3%81%AE%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88)いかにして集客するか？SEO 対策のポイント

ここでは、Google 検索からの流入を増やすための **SEO 対策** についてお話しします。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%AA%E3%81%9C-seo-%E3%81%8C%E9%87%8D%E8%A6%81%E3%81%AA%E3%81%AE%E3%81%8B)なぜ SEO が重要なのか

SEO とは簡単に言うと「Google 検索で上位に表示されるようにするための対策」です。

個人開発をしていると、「せっかく作ったサービスをどうやって多くの人に見つけてもらうか」が大きな課題になります。

**がんばって作ったのに誰も使ってくれない...** という経験をした方もいるのではないでしょうか？

そのため、継続的に人を呼び込むには、検索エンジンで上位に表示される工夫が欠かせません。

このような対策を **SEO（Search Engine Optimization）** といいますが、当サイトでは SEO 対策にかなり力を入れています。

業務の中で覚えてきた SEO 対策のノウハウを活かし、サイト内の構造やコンテンツ、メタデータなどを最適化しています。

他の個人開発者にも役立ちそうなところをいくつか紹介しておきます！

-   **検索キーワードの調査**
-   **構造化データの設定**
-   **メタデータの設定**
-   **パンくずリストの実装**
-   **Core Web Vitals の最適化**
-   **ページ読み込み速度の最適化**
-   **モバイルフレンドリー対応**

それぞれ、考え方や使ったツールを紹介しますので、参考にしてみてください。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#seo-%E5%AF%BE%E7%AD%96%E3%81%AE%E6%88%90%E6%9E%9C)SEO 対策の成果

色んな SEO 対策を行った結果、ほぼ宣伝なし＆新規ドメインであるにもかかわらず、 **Google 検索からの流入が増加** し続けています。

[![pv.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F8ca95d3f-42a2-410a-b233-5875ee855258.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0cf71948e62d04a72f65577b136b8255)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F8ca95d3f-42a2-410a-b233-5875ee855258.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0cf71948e62d04a72f65577b136b8255)

これは本当に **すごーく頑張った成果** だと思っていますが、具体的にどのような工夫をしたかを紹介します。

特別なことはしておらず、色んな観点で **地道な SEO 対策** を行った結果ですので、皆さんの個人開発でも参考になるかと思います！

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E6%A4%9C%E7%B4%A2%E3%82%AD%E3%83%BC%E3%83%AF%E3%83%BC%E3%83%89%E3%81%AE%E8%AA%BF%E6%9F%BB)検索キーワードの調査

まず、考え方からですが、 **検索キーワードの調査** が一番大事だと考えています。

なりふり構わず「自分が好きなアプリ」を作っても、残念ながら需要がなければ、検索エンジンからの流入が少ない場合があります。

そのため、ユーザーが検索するキーワードのボリュームを調査し、それに合わせたコンテンツを提供することが大事です。

このあたりは日常業務や技術ブログでの経験が活かした部分ですが、 [ラッコキーワード](https://rakkokeyword.com/) というツールを使っています。

たとえば、ラッコキーワードを使うと、**あるキーワードに対してどれくらいの検索ボリューム** だったり、競合サイトの強さなどがわかります。

[![スクリーンショット 2025-03-11 13.31.34.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F11b3c586-cab0-456b-8e6c-ef8ad95abca5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4dc21f92f89834793c17b0412ac109e5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F11b3c586-cab0-456b-8e6c-ef8ad95abca5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4dc21f92f89834793c17b0412ac109e5)

Learning Next においては「Udemy Python おすすめ」や「React 学習 ロードマップ」など、プログラミング学習に関するキーワードの検索上位を狙ってコンテンツを作成しています。

ただし、ユーザーの需要が多いキーワードは競合も多いため、ラッコキーワードの **「SEO 難易度」** という指標を参考にしています。

他にも実際にキーワードで検索したときに、 **大企業や大手 Web サイトが上位に表示されている場合は、正直個人では勝ち目が薄いので避ける** 方がベターです。

[![スクリーンショット 2025-03-18 8.41.50.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F7553a6aa-bd7b-445f-82de-8ee699ab815c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5ba7a53622572f77c7f56b13748e5500)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F7553a6aa-bd7b-445f-82de-8ee699ab815c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5ba7a53622572f77c7f56b13748e5500)

これから Web アプリやサイトを立ち上げる方は、数個の検索キーワードについて **Google 検索上位を狙う** という目標を持つと、具体的な方向性が見えてくるかもしれませんね。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E6%A7%8B%E9%80%A0%E5%8C%96%E3%83%87%E3%83%BC%E3%82%BF%E3%81%AE%E8%A8%AD%E5%AE%9A)構造化データの設定

ブログやコースの一覧ページなどは、Google などの検索エンジンが理解しやすい形で **構造化データ（JSON-LD）** を埋め込んでいます。

皆さんも Google 検索をしたとき、検索結果に「星評価」や「料金」が表示されていることがありますよね。

[![スクリーンショット 2025-03-11 13.34.37.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fd580666f-6571-4328-adfc-6464d18f6ff0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d7d586d519e68fb9e4b4c8a33adc11e6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fd580666f-6571-4328-adfc-6464d18f6ff0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d7d586d519e68fb9e4b4c8a33adc11e6)

必ず表示されるわけではないですが、表示されると情報が充実し、ユーザーがクリックしやすくなります。

Learning Next でも、コース一覧や講座詳細ページに構造化データを設定しています。

[![スクリーンショット 2025-03-11 13.36.33.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fa5d2c8e2-541f-4fc3-bc67-49748c0ca7c0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2e02754eec377ba8aa674db90c19e053)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fa5d2c8e2-541f-4fc3-bc67-49748c0ca7c0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2e02754eec377ba8aa674db90c19e053)

結果的に、クリック率が上がり、検索エンジンからの流入が増えることが期待できます。

-   参考：[Google 検索における構造化データのマークアップの概要](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data?hl=ja)

↑ 公式ドキュメントながら、わかりやすい解説がされているので、SEO 対策に興味がある方はぜひ参考にしてみてください。

なお、構造化データは Google のボットがサイトをクロールする際に、ページの内容を理解しやすくするためのものです。

そのため、作ったデータ形式にミスがあると認識されなくなってしまうので、[リッチリザルトテスト](https://search.google.com/test/rich-results?hl=ja) というツールを使って、正しく構造化データが設定されているか確認することをおすすめします。

デプロイした URL を貼ってもいいですし、ブラウザ上からデベロッパーツールで確認したソースコードを貼り付けてもチェックできます。

[![スクリーンショット 2025-03-11 13.39.14.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F32a3fae8-5f61-4d80-9a6e-b155f7ce3ea7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=221b189454628845bf679c12af5402b2)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F32a3fae8-5f61-4d80-9a6e-b155f7ce3ea7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=221b189454628845bf679c12af5402b2)

なお、Google Search Console に登録しておくと、構造化データのエラーがあった場合に通知が来るので、ミスを見逃しにくくなります。

[![スクリーンショット 2025-03-18 8.46.58.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fc0228b5a-c6b4-49ed-a9d5-f4babf026ccb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c3af09f213ea25656ba483ff329a38bb)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fc0228b5a-c6b4-49ed-a9d5-f4babf026ccb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c3af09f213ea25656ba483ff329a38bb)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%A1%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF%E3%81%AE%E6%9C%80%E9%81%A9%E5%8C%96)メタデータの最適化

次に **メタデータの最適化** についてです。

メタデータとは簡単にいうと、検索エンジンに表示される「タイトル」や「説明文（メタディスクリプション）」のことです。

他にも、X (Twitter) や OGP (Facebook) といったメタデータもあり、これらを最適化することで、検索結果や SNS でのシェア時に表示される情報をコントロールできます。

デベロッパーツールでページの要素を見たときに `<meta xxx>` と表示されている情報のことですね。

[![スクリーンショット 2025-03-11 13.40.53.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fff4a2a09-4b8b-47db-97ef-cf13839b73d9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=eae482c21b6e50167e525b654426e641)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fff4a2a09-4b8b-47db-97ef-cf13839b73d9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=eae482c21b6e50167e525b654426e641)

タイトルや説明文が検索結果に出たときにユーザーに興味を持ってもらえるよう、設定しておくのがおすすめです。

-   参考：[メタタグ（meta タグ）とは？SEO への影響と重要な 7 つのタグの最適化について解説](https://emma.tools/magazine/internal-measures/meta-tags-for-seo/)

Learning Next ではページごとに動的に（固定ではない形で）メタデータを設定できるようにしていますが、面倒だったら固定でもいいかなとも思っています。

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%91%E3%83%B3%E3%81%8F%E3%81%9A%E3%83%AA%E3%82%B9%E3%83%88%E3%81%AE%E5%AE%9F%E8%A3%85)パンくずリストの実装

訪問者がサイト内でどの位置にいるのかを示すために、**パンくずリスト** も実装しています。

パンくずとは、サイトの階層構造を示すリンクのことです。

[![スクリーンショット 2025-03-11 13.45.41.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F801837d8-5348-4005-9737-a1829606a4e3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=229935a1af9e2fb956d54a30b6facf24)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F801837d8-5348-4005-9737-a1829606a4e3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=229935a1af9e2fb956d54a30b6facf24)

たとえば `ホーム > Udemy講座 > 講座詳細 > 講座名` のように設定し、サイト内の現在地をユーザーに教えるために使えます。

これがあるとユーザーが使いやすいだけでなく、サイトの構造を Google のボットが認識しやすくなるので、SEO にも効果的です。

こうした小さな工夫の積み重ねが、最終的には検索エンジンに好まれるサイトへとつながると思っています。

個人開発だからこそ、細かいところまで自分のペースで作り込めるのが醍醐味ですね。

-   参考：[パンくずリストとは？種類・設置方法・SEO メリットを解説](https://mieru-ca.com/blog/what-is-breadcrumbs/)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#core-web-vitals-%E3%81%AE%E6%9C%80%E9%81%A9%E5%8C%96)Core Web Vitals の最適化

ここ数年、Google は **Core Web Vitals** という指標を重視しています。

すごーく簡単に言うと、 **ユーザーにとって使いやすいページかどうか** を測る指標です。

-   参考：[Core Web Vitals と Google 検索の検索結果について](https://developers.google.com/search/docs/appearance/core-web-vitals?hl=ja)

たとえば、以下のような指標があります。

-   **ページの読み込み速度**
-   **初期表示までの時間**
-   **ページのレイアウトの安定性（ロード完了後にコンテンツがどれだけ動くか）**

どれもこれもユーザー体験に直結する指標であり、Google では検索結果に表示されるページのランキングにも影響を与えるとされていますので、ここは重要なポイントです。

試しに、自分のサイトを [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=ja) でチェックしてみると、どの部分が改善の余地があるかがわかります。

たとえば、[フロントエンドエンジニア(React)を目指す学習ロードマップ](https://learning-next.app/roadmap/frontend-engineer-react) というページで見てみると、以下のような情報が出ます。

はっきりと **合格** か **不合格** かが出るので、意外とわかりやすいものです。

[![スクリーンショット 2025-03-11 13.48.07.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F5263a532-1a32-4a32-9105-abebd1732992.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fdc60af729cc680128f4ee741753747c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F5263a532-1a32-4a32-9105-abebd1732992.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fdc60af729cc680128f4ee741753747c)

開発中であるなどの理由でローカルでしか動かせないのであれば、\*\*LightHouse という Chrome の拡張機能、またはデベロッパーツールからもチェックできます。

-   参考：[Lighthouse とは？Google 公式 SEO ツールの機能と改善法を解説！](https://blog.microcms.io/what-is-lighthouse/)
-   参考：[Lighthouse の概要](https://developer.chrome.com/docs/lighthouse/overview?hl=ja)

中身としては PageSpeed Insights とほぼ同じです。

別なページで計測してみました。

[![スクリーンショット 2025-03-11 11.46.50.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fef619b45-0646-4948-9cd9-152f37e343c8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0b9ec0e8391535285e6e313dfc2885c8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fef619b45-0646-4948-9cd9-152f37e343c8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0b9ec0e8391535285e6e313dfc2885c8)

ちなみに「パフォーマンス」という項目もいくつかの指標に分かれており、Learning Next の一部ページだと **FCP(Fist Contentful Paint)** という指標が黄色信号になっていますね。

これは「初期表示までの時間」を示す指標で、ユーザーが最初のコンテンツを見るまでの時間が長いと、その間に離脱率が上がる可能性があるため、改善が必要とわかります。

リリース後、**丸々 1 ヶ月ぐらい** はこれら Core Web Vitals の指標を改善するために努力しました。

正直、機能追加やコンテンツ作成より、この作業が一番時間がかかりましたが、その分検索エンジンからの流入が増えたので、やってよかったと思っています。

なお、[Google Search Console](https://search.google.com/search-console/about) にサイトを登録しておくと、問題があるページを教えてくれるので、こちらもおすすめです。

[![スクリーンショット 2025-03-18 8.45.34.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Ff625a421-d50b-4c8e-b213-8214659a905c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=157b355147811dcf7edfdf5ee31f3658)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Ff625a421-d50b-4c8e-b213-8214659a905c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=157b355147811dcf7edfdf5ee31f3658)

ちなみに Vercel だと、1 つパッケージを入れるだけで、実際にユーザーアクセスに基づき、ページごとに継続監視できます。

React や Next.js で開発している方は、入れてみるといいでしょう。

-   参考：[vercel の Web Analytics を Next.js のプロジェクトに導入する](https://zenn.dev/k_neko3/articles/9e716018ab8717)

[![スクリーンショット 2025-03-11 13.53.47.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F657813cc-53f4-4ca7-83c5-5310040b0b10.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c04bd7a2939ac5d1a47c9c4499ea60fb)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F657813cc-53f4-4ca7-83c5-5310040b0b10.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c04bd7a2939ac5d1a47c9c4499ea60fb)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%9A%E3%83%BC%E3%82%B8%E8%AA%AD%E3%81%BF%E8%BE%BC%E3%81%BF%E9%80%9F%E5%BA%A6%E3%81%AE%E6%9C%80%E9%81%A9%E5%8C%96)ページ読み込み速度の最適化

Core Web Vitals の一部にもなっている「ページの読み込み速度」ですが、これはユーザー体験に直結する指標です。

ページが遅いと、ユーザーは離脱してしまう可能性が高まりますので、できる限り速くすることが大事なのは言うまでもありません。

Learning Next では大量のデータを扱う中、以下のような工夫をしてページ読み込み速度を向上させています。

-   **優先的に表示したいコンテンツだけ最初に読み込む**
-   **初期表示で見える範囲だけ最初に読み込む**
-   **キャッシュを活用して再読み込み時の速度を向上させる**
-   **Next.js の SSG を使って事前にページを生成しておく**

特に、**コンテンツの読み込みを遅延** させることで、初期表示までの時間を短縮する工夫が効果的だったと思います。

難しく聞こえるかもしれませんが、言うなれば「ちょっとずつ表示させる」工夫です。

-   参考：[【初学者向け】React 超簡単なパフォーマンス改善方法](https://zenn.dev/kao126/articles/8a438fd8120105)

JavaScript の知識が必要になりますが、React や Next.js などのフレームワークを使うと、このあたりの最適化が比較的簡単にできるのでおすすめです。

ただし、適当に実装すると、 **読み込み完了時に画面が崩れる** などの問題が発生することもあり、結果的に UI/UX が悪化することもあるので、慎重に対応することが大事です。

そのため、読み込み中にも要素の高さを確保する CSS を適用したり、読み込み中のアニメーションを表示するなど、ユーザーに対してわかりやすい対応を心がけています。

-   参考：[CLS とは？スコアが悪化する 3 つの原因と改善方法 3 選](https://anymanager.io/ja/blog/cls)

### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E3%83%95%E3%83%AC%E3%83%B3%E3%83%89%E3%83%AA%E3%83%BC%E5%AF%BE%E5%BF%9C)モバイルフレンドリー対応

最後に、地味に意識しておきたいのが **モバイルフレンドリー対応** です。

簡単に言うと、「スマホでも快適に閲覧できるようにする」ということです。

Google では 2018 年から、モバイルファーストインデックスという指標を導入しており、スマホで快適に閲覧できるかどうかが検索順位に影響を与えるようになっています。

-   参考：[モバイル ファースト インデックスの展開](https://developers.google.com/search/blog/2018/03/rolling-out-mobile-first-indexing?hl=ja)

昨今、スマホで閲覧する人も多いので、モバイルフレンドリー対応は必須と言えるでしょう。

（自分が何年も前に Ruby on Rails で作ったポートフォリオは、今思えばスマホでは文字が小さく、メニューもまともに操作できないような状態でした...）

具体的な考え方などは、Google 公式を参考にしています。

-   参考：[モバイルサイトとモバイルファースト インデックスに関するおすすめの方法](https://developers.google.com/search/docs/crawling-indexing/mobile/mobile-sites-mobile-first-indexing?hl=ja)

慣れていない方は、まずは **レスポンシブデザイン** だけでも始めるといいかもしれません。

これは、スマホやタブレットなど、さまざまなデバイスに対応できるデザインのことです。

たとえば、Learning Next では、パソコンとスマホで一部表示が変わるように設定しています。

また、スマホ特有の操作性を考慮してこだわったポイントがあるので、参考になればと思い詳しくご紹介します。

#### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%82%A2%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%83%B3%E3%81%A7%E3%82%B9%E3%83%9A%E3%83%BC%E3%82%B9%E3%82%92%E6%9C%89%E5%8A%B9%E6%B4%BB%E7%94%A8)アコーディオンでスペースを有効活用

あまり長い文章が最初から表示されると、スマホ画面ではスクロールが必要になります。

その場合、スクロールが面倒になる上、全貌がわからず離脱される可能性があるため、アコーディオンを使って折りたたみ表示にしています。

**アコーディオンが閉じている時：**

[![スクリーンショット 2025-03-11 13.57.16.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6e5baef6-d6fa-4eb7-9be0-dbd431e7b139.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=16a96e0bea96a0c2a2f264a9aba9ff6c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F6e5baef6-d6fa-4eb7-9be0-dbd431e7b139.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=16a96e0bea96a0c2a2f264a9aba9ff6c)

**アコーディオンを開いた時：**

[![スクリーンショット 2025-03-11 13.57.58.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fe4d0dd84-49f0-489e-bad7-8c3149443490.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e8b15b35dd152ee397980a3c7a73cf08)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fe4d0dd84-49f0-489e-bad7-8c3149443490.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e8b15b35dd152ee397980a3c7a73cf08)

#### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%83%95%E3%83%AD%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%9C%E3%82%BF%E3%83%B3%E3%81%A7%E3%82%B9%E3%83%A0%E3%83%BC%E3%82%BA%E3%81%AA%E7%A7%BB%E5%8B%95)フローティングアクションボタンでスムーズな移動

先述の通り、あまりにも長いページだとスクロールが面倒になります。

そのため、一部のページでは「フローティングアクションボタン」という、右下に固定されたボタンを設置しています。

[![スクリーンショット 2025-03-11 13.58.44.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F7be705fd-76c6-457a-be20-b381b57b4c01.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bf84e2c1b099604555172d2eeb05670e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F7be705fd-76c6-457a-be20-b381b57b4c01.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bf84e2c1b099604555172d2eeb05670e)

押すと、このように目次が開き、スマホでも快適に移動できるようになっています。

[![スクリーンショット 2025-03-11 13.59.33.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fed785746-517a-4caa-9e84-e0cc5f0b6d99.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c6a5efef1032810ad7b7f38137793f90)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fed785746-517a-4caa-9e84-e0cc5f0b6d99.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c6a5efef1032810ad7b7f38137793f90)

こういった、ユーザーの操作をちょっと便利にする機能も、モバイルフレンドリー対応の一環として取り入れています。

また、最初からあえて出さず、スクロールして一定以上移動したときに表示されるボタンもあります。

**初期表示時：**

[![スクリーンショット 2025-03-11 14.00.46.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F441d25a2-664d-416f-b6e4-6a2f680c5d6b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6eb08b9c546319e909c33a6a0a40ff25)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F441d25a2-664d-416f-b6e4-6a2f680c5d6b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6eb08b9c546319e909c33a6a0a40ff25)

**スクロール後：**

[![スクリーンショット 2025-03-11 14.00.57.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F73035fbd-3cb1-4223-90f9-eeaa4958434c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=68b26731142fd5906de3fca44f1a8174)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F73035fbd-3cb1-4223-90f9-eeaa4958434c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=68b26731142fd5906de3fca44f1a8174)

これは、**ある程度コンテンツを読んでから行なって欲しいアクションを促すための工夫**です。

最初から「見てみて！」とアピールするとうるさく、敬遠されてしまうかもしれません。

そのため、興味を持ってコンテンツを読み、スクロールしてくれたユーザーにだけ表示されるようにしています。

#### [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%82%B5%E3%82%A4%E3%83%89%E3%83%90%E3%83%BC%E3%82%92%E3%83%89%E3%83%AD%E3%83%AF%E3%83%BC%E3%81%AB%E3%81%97%E3%81%A6%E7%B8%A6%E9%95%B7%E7%94%BB%E9%9D%A2%E3%81%AB%E5%90%88%E3%82%8F%E3%81%9B%E3%82%8B)サイドバーをドロワーにして縦長画面に合わせる

Udemy 講座をトピック別に検索する際、パソコンではサイドバーに「受講生数」「評価」などでのフィルタや、ソート機能を表示しています。

[![スクリーンショット 2025-03-11 14.02.29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fc1e64db5-00ed-40b1-a66e-776d44a3b0e0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c46b80ece63a0656839978b98ed42c73)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fc1e64db5-00ed-40b1-a66e-776d44a3b0e0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c46b80ece63a0656839978b98ed42c73)

しかし、スマホで同じく横に表示したり、常時表示するとスペースを占有してしまうので、押すと開く **ドロワーメニュー** にしています。

[![スクリーンショット 2025-03-11 14.03.17.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F37ff6424-dc7e-4cae-b8ab-2790581d3c89.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cce2157a6c105b7383fc902bfc96f7d8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F37ff6424-dc7e-4cae-b8ab-2790581d3c89.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cce2157a6c105b7383fc902bfc96f7d8)

折りたたみにも通ずるところですが、ドロワーメニューはスマホ画面に合わせて、スペースを有効活用できるのでおすすめです。

（ただし、ドロワーメニューは使いすぎるとユーザーが見落とす可能性もあるので、使いどころを考える必要があります）

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E9%81%8B%E7%94%A8%E3%82%B3%E3%82%B9%E3%83%88%E3%81%AF%E3%81%A9%E3%81%86%E3%81%AA%E3%81%A3%E3%81%A6%E3%82%8B)運用コストはどうなってる？

Web アプリは自分で環境を用意してあげる必要があるので、運用コストも気になるところですよね。

本アプリはメインは Next.js で構築しているため、 **Vercel** でデプロイしています。

データベースには [Supabase](https://supabase.com/) の無料プランを使っています。

-   参考：[Supabase とは？主な機能や Firebase との違い、使い方を解説](https://udemy.benesse.co.jp/development/app/supabase.html)

無料プランだと自動バックアップが無いなど制限はあるものの、そこは自分でバックアップを取るなどの対応をすれば問題ないと判断しました。

ちなみに、Vercel は **Pro プラン** を使い、月額 $20 ほどのコストがかかっています。

-   参考：[Vercel Pricing](https://vercel.com/pricing)

[![スクリーンショット 2025-03-18 8.48.49.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F37b26551-6eda-43ee-82f4-a1be591a2ac5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2fec872adda61211d027a22b2f6cbfb1)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2F37b26551-6eda-43ee-82f4-a1be591a2ac5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2fec872adda61211d027a22b2f6cbfb1)

趣味で使う分には無料の Hobby プランでも十分ですが、収益化するアプリには使えない規約があるため、Pro プランを選択しました。

それでも、月額 $20 は十分にコストを取り戻せる範囲だと考えているので、今後も継続していく予定です。

また、一部 API には Ruby on Rails を使っているため [Render.com](https://render.com/) でデプロイしています。

[Neon](https://neon.tech/) という無料データベースと組み合わせ、無料で運用できているので、バックエンドにはコストがかかっていません。

-   参考：[【2025 年版】Render.com と Neon で Rails アプリを無料デプロイする方法](https://qiita.com/tomada/items/8be6e0df7ad74ef59207)

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%8A%E3%82%8F%E3%82%8A%E3%81%AB)おわりに

個人開発は、アイデアを形にする楽しさや、自分のペースで作業できる自由さがあります。

しかし、一方で **誰も使ってくれない...** という、新たな課題も抱えることがあります。

それでも **SEO 対策** などの工夫をすることで、検索エンジンからの流入を増やすことができることが分かったかと思います。

好評であれば、よりノウハウを詳しく解説したり、他の個人開発者の方にも活かせるような情報を提供していきたいと考えています！

サービスに関するフィードバックや感想は大歓迎です！

[X (Twitter)](https://x.com/muscle_coding) や、Qiita のコメントなどでお知らせいただけると嬉しいです。

最後までお読みいただき、ありがとうございました！

参考になるところがあれば、ぜひ「いいね」やシェアをしていただけると嬉しいです！！！！！！

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E4%BB%98%E9%8C%B2-%E4%BB%8A%E3%81%BE%E3%81%A7%E5%AD%A6%E3%82%93%E3%81%A7%E3%81%8D%E3%81%9F%E4%B8%AD%E3%81%A7%E7%89%B9%E3%81%AB%E5%BD%B9%E7%AB%8B%E3%81%A3%E3%81%9F-udemy-%E8%AC%9B%E5%BA%A7)付録： 今まで学んできた中で特に役立った Udemy 講座

せっかくなので、未経験からのエンジニア転職〜個人開発までに使ってきた Udemy 講座（のレビュー分析）を紹介します。

ここまでのレベルに至るまでに特に役立った、おすすめの講座だけをピックアップしています。

-   [【JS】ガチで学びたい人のための JavaScript メカニズム](https://learning-next.app/udemy/courses/javascript-essence)
-   [超 TypeScript 完全ガイド 2025](https://learning-next.app/udemy/courses/typescript-complete)
-   [React(v18)完全入門ガイド｜ Hooks、Next14、Redux、TypeScript](https://learning-next.app/udemy/courses/react-complete-guide)
-   [React Hooks を完全に理解する Hooks マスター講座【React18~19 対応】](https://learning-next.app/udemy/courses/react-hooks-complete-course)
-   [はじめての Ruby on Rails 入門-Ruby と Rails を基礎から学びウェブアプリケーションをネットに公開しよう](https://learning-next.app/udemy/courses/the-ultimate-ruby-on-rails-bootcamp)
-   [現役シリコンバレーエンジニアが教える Python 3 入門 + 応用 +アメリカのシリコンバレー流コードスタイル](https://learning-next.app/udemy/courses/python-beginner)

全部を受講する必要はないですが、自分に足りない部分を補うために、必要な講座を選んでいただくと良いかもしれません。

## [](https://qiita.com/tomada/items/b992245a4162ddeb1f6e#%E3%81%A1%E3%82%87%E3%81%A3%E3%81%A8%E5%AE%A3%E4%BC%9D%E3%82%AA%E3%83%AA%E3%82%B8%E3%83%8A%E3%83%AB-udemy-%E8%AC%9B%E5%BA%A7%E3%81%AE%E3%82%AF%E3%83%BC%E3%83%9D%E3%83%B3%E9%85%8D%E5%B8%83%E4%B8%AD)ちょっと宣伝：オリジナル Udemy 講座のクーポン配布中！

本業のかたわら **プログラミングスクールのメンターとして数百名を指導** してきました。

そんな中、**「もっと多くの人にプログラミングを学んでもらいたい」** という思いから、オリジナルの Udemy 講座を作成しています。

それぞれ **1,500 円** で受講できるクーポンを配布していますので、ぜひご利用ください！

-   [当サイト限定！オリジナル Udemy 講座の特別割引クーポン一覧](https://learning-next.app/coupons)

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fba1e4238-e613-4570-bd70-a29009ccb5b4.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5285c5ba3d2e7775c52f63cec736b913)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F364501%2Fba1e4238-e613-4570-bd70-a29009ccb5b4.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5285c5ba3d2e7775c52f63cec736b913)