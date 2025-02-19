---
finished reading: false
favorite: false
tags:
  - clippings
---
# 【イベントレポート/LT #1】Honoで実現するバックエンド開発の今 #Offers_DeepDive ｜#Offers_DeepDive
  #ReadItLater 
 #ReadableArticle

## articleURL
https://note.com/offers_deepdive/n/n635cd473675c#18a71f6b-7179-4d22-b75e-cbfb677ee5ff

## siteName
note（ノート）

## date
2025-02-14

## articleContent
2024年10月22日、株式会社overflowが主催するイベント「どこまで安定してる？Express/NestJS/Hono運用者に聞く バックエンドTSのイマ」がオンラインにて開催されました。

▼connpass詳細はこちら

  
▼アーカイブはこちら

## 登壇者紹介

### 【Hono】SRE sugar-cat/石井俊輝 氏（[@sugar235711](https://x.com/sugar235711)）

新卒でSIer→CyberAgent→???(所属非公開) アプリケーション開発からインフラの運用まで何でもやる人。セキュリティやパフォーマンスチューニングの話が好き。

### 【Express】RPGテック合同会社 エンジニア 伊藤 康太 氏（[@koh110](https://x.com/koh110)）

元ヤフー情シス/CTO室/黒帯（Webフロントエンド)。現RPGテック合同会社でスタートアップ立ち上げやアドバイザーなど

### 【NestJS】株式会社メドレー テックリード 今給黎 成彬 氏

株式会社メドレー テックリード。2022年にM&Aを通じてメドレーへ入社。医療・福祉業種向けのオンライン動画研修サービス「ジョブメドレーアカデミー」でバックエンド・フロントエンド・モバイルアプリの開発を担当。

**大谷：**本日モデレーターを務めます大谷と申します。このイベントを開催している株式会社overflowで取締役CTOを務め、開発周りの監視をしています。  
本日のイベントでは、「バックエンドTSのイマ」をテーマに、TypeScriptを用いたバックエンド開発の活用方法や安定性について掘り下げていきます。

最近では、フロントエンドやバックエンド、さらにはインフラ構築までをTypeScriptで統一する構成を選択するチームが増えてきたと耳にすることが多くなりました。「Full TypeScript」というワードも登場し、注目を集めつつあるように感じます。  
TypeScriptで統一することで、型情報を共有して開発効率を向上させたり、コンテキストスイッチを最小化したりと、多くのメリットを感じられる場面があります。しかし、長期的な運用を見据えたバックエンド開発におけるTypeScriptの活用については、実態に関する知見がまだ十分に出そろっているとは言えない状況だと思います。  
そこで本日は実務での運用経験の豊富な3名の登壇者をお招きしてExpress、Next.js、Honoという異なるフレームワークの特長も交えながらTypeScriptを用いたバックエンド開発の実態とその可能性についてお話いただく会となっています。

私は後半のディスカッション・質疑応答でモデレートを担当いたしますのでよろしくお願いいたします。

## LT：Honoで実現するバックエンド開発の今｜sugar-cat/石井俊輝 氏

**sugar-cat/石井 俊輝 氏：**私からは「Honoで実現するバックエンド開発の今」をテーマに発表させていただきます。

![](https://assets.st-note.com/img/1738650222-aqdBFLEmUXp9zINVwkuHW0xJ.png?width=1200)

sugar-catこと石井と申します。今はSREのようなことをしていて、普段はオブザーバビリティの最適化や構築、インフラ支援をしています。

Honoに関しては、先日開催されたHonoカンファレンスやブログなどにいろいろ書いているので、もし本日のLTを聞いて興味を持ったらぜひ見てみてください。

![](https://assets.st-note.com/img/1738650256-wCyV6SspOgtMmqerUWcdJoIb.png?width=1200)

本日の内容です。バックエンドTS特有の難しさと、Honoを絡めたバックエンド開発はどのようなものか、というようなお話をしていきます。

![](https://assets.st-note.com/img/1738650289-2oyJ5bspltChuaOe1IQU3gTG.png?width=1200)

はじめにこの発表の対象者は、これからバックエンドTSをやって行きたい方やHonoが気になる方、運用で気をつけたいことを知りたい方に向けた発表となります。

### 1\. バックエンドTS特有の難しさ

![](https://assets.st-note.com/img/1738650346-DOwp7t5LCIfiJdlxn8aVQP4b.png?width=1200)

最初にバックエンドTypeScriptの難しさについてお話します。特に他言語出身の方にとって難しいと感じるのは、広大なエコシステムです。ランタイム、ハンドラー、パッケージマネージャなど、さまざまなツールをキャッチアップしないと、そもそも開発が始められない状態に陥ることがあります。

デファクトのようなツールは多く存在しますが、「どれを選べば良いのか」がわからないと思います。さっと挙げるだけでも、これらのツールを知らないと開発が進められない状態だと感じます。

![](https://assets.st-note.com/img/1738650381-bu1LBKneQZ3mXTHUpAt6DY7O.png?width=1200)

結局、「何を基準に選べば良いのか」が分からないので、まずは一般的なAPIを作るという前提で、何を選定すれば良いのかを考えていきます。

![](https://assets.st-note.com/img/1738650401-lHArf3geqpd6TwXVL2OGSyvx.png?width=1200)

最初にライブラリやフレームワークを選ぶのではなく、まず作りたいサービスに合ったインフラを選定することになると思います。

![](https://assets.st-note.com/img/1738650420-VgYTIky3MZumoi6vqQSH9rsf.png?width=1200)

そして、そのインフラ上で動作するサービスを使う場合には、どのランタイムが対応するイメージがあるのかを見ていくと良いと思います。

![](https://assets.st-note.com/img/1738650443-2F8g9fLQV7hB1wuCtKlk6oGp.png?width=1200)

その上で、アプリケーションを書く際には、公式SDKが使えるかどうかや、バージョンがしっかりアップグレードされているかを確認していきます。

![](https://assets.st-note.com/img/1738650463-7C0aYUuDbE5QFksHdqIfeRiA.png?width=1200)

そしてバックエンドだけの話ではないので、フロントエンドやAIのチームと共同で作業しやすく、スキーマを用いて会話できるかどうかも気にする必要があると思います。

それぞれ気にしなければならないことが多い中で、「これらをいろいろ吸収して、いい感じにまとめてくれるフレームワークなんてあるの？」と思いますよね。そこで紹介したいのが「Hono」です。

![](https://assets.st-note.com/img/1738650489-bu4Epz5sta9nIe1KiFSJ2Ayo.png?width=1200)

Honoは、Web標準に基づいて軽量かつ高速で、どんなJavaScriptランタイム上でも動作するWebフレームワークです。  
特長としては、実行環境の差異を吸収する「Adapter」、標準APIを扱いやすくする「Helper」、さらにWebアプリケーション開発に必要な便利な機能を詰め込んだ「Middleware」などを備えた非常に使いやすいフレームワークです。

![](https://assets.st-note.com/img/1738650512-76h8uRrvgcVfyYz9Z2IwJFCa.png?width=1200)

例えば先ほどの例に当てはめると、ランタイムに依存する部分もHonoを使えばほぼ気にせずに済みます。

![](https://assets.st-note.com/img/1738650533-N0nZojqxJQWAsgD4X8MOr5Ty.png?width=1200)

ホスティングするサービスは、Honoの「Adapter」を活用することで実行環境の差異を吸収でき、同じ書き方ができます。

![](https://assets.st-note.com/img/1738650551-qITecvH92MopiRAO1CwKyEkD.png?width=1200)

また、ミドルウェアが充実していることに加え、ビルドインのものも充実しています。さらに、サードパーティもいろいろな種類があるので、それらを組み合わせればほぼすべてのクラウドプロバイダにも対応できるようになっています。

![](https://assets.st-note.com/img/1738650570-4cxKDgizLrvFe3hX01ykApdH.png?width=1200)

Honoの構成要素について詳しく話す時間は限られていますが、興味があれば調べてみてください。

### 2\. HonoとTypeScriptバックエンド開発

![](https://assets.st-note.com/img/1738650624-Ud8xe2mFQTZcG3fq45V6ulBp.png?width=1200)

次に、Honoとバックエンドを使った実装パターンを見ていきます。  
まず、BFFの部分でHonoをどのように使うかについてお話します。

![](https://assets.st-note.com/img/1738650641-Evcs7VNSt3RmnJPWwOHZ9rdG.png?width=1200)

最近のBFFにはさまざまな選択肢がありますが、有名どころとしてNext.jsやGraphQL Serverが話題になることが多いと思います。

![](https://assets.st-note.com/img/1738650660-J6z5WA4toZfVvSjQL1MG7knx.png?width=1200)

今日は例として、Next.jsとHonoをどのように組み合わせて使うかについて説明します。

Next.jsはご存じの通り、API RouterやApp RouterのRSC（React Server Components）など、BFF相当の機能を実装することが可能です。ここにHonoをVercelの「Adapter」と組み合わせることで、Honoのシンプルな書き方でBFFを開発することができます。

Pages Router/App Router共にAPI Routes上でHonoを使用することができます。

![](https://assets.st-note.com/img/1738650695-FKD8kTms9N6L3WSqI0pcjGPn.png?width=1200)

次にこちらがメインのお話ですが、バックエンドサーバでのHonoの使い方についてです。

![](https://assets.st-note.com/img/1738650713-kMc0CpAXlRHoFjxItBfiU8Vh.png?width=1200)

バックエンドと言っても、マイクロサービスの一部としてのバックエンドや、単純なクライアントサーバ間のAPIを提供するバックエンドなど、さまざまな形態があると思います。今回は単純にクライアントブラウザからアクセスされるサーバのバックエンドとしての話をします。

![](https://assets.st-note.com/img/1738650733-l3DTLMFifUPWz7EVcSk1jont.png?width=1200)

Honoで開発を進める場合、私の中では大きく2つの方法が考えられます。ひとつは「Zod OpenAPI」を組み合わせてHonoのSAPIを記述する方法です。この「Zod OpenAPI」をラップしたサードパーティ製のミドルウェアにhono/zod-openapiなどがあり、それを活用することでスキーマからAPIを効率的に作成することができます。  
また、Swagger UIのようなミドルウェアを組み合わせることで、このようにUIを自動生成できる機能も提供されます。

![](https://assets.st-note.com/img/1738650754-pqO6r7kcTQg5IitjLuPBf2KD.png?width=1200)

ZodをベースにRequest/Responseのスキーマを定義し、

![](https://assets.st-note.com/img/1738650855-nqZTmKSRfl9obwdj5Q4Gz6he.png?width=1200)

それをHonoのインスタンスにルートとしてアタッチすることで、

![](https://assets.st-note.com/img/1738650880-IPkNcXoCAOpr6nv0hBmF8YE4.png?width=1200)

生成されたJSONを基にURLを自動生成する仕組みがHonoでサポートされています。

![](https://assets.st-note.com/img/1738650899-daM0wfv864DetQRSFuiW3TZk.png?width=1200)

もうひとつは「Hono RPC」を使用する方法です。これはtRPCと同様のことができる機能で、実装者としてはただREST APIを記述するだけです。  
その後、作成したRouteの型をhono/clientに渡すことで、型補完の恩恵を受けながら開発を進めることが可能です。この方法はバックエンドのBFFはもちろん、他のあらゆる場面でも活用することができます。APIを作成するだけでtRPCに似た機能を実現できる点が特徴です。

### 3\. 運用について

ここまでの説明で「Honoは便利だ」「使ってみたい」と感じた方もいるのではないでしょうか。  
ここからは、実際にHonoやTypeScriptを使用して運用する際に気を付けるポイントを簡単にご紹介します。

![](https://assets.st-note.com/img/1738650945-AGC16hSz2V4ncmkytUL0F8TJ.png?width=1200)

開発はゴールではなく、ユーザーが利用を開始し、エンジニアが改善のサイクルを回せるようになってからが本番です。しかし、「運用では具体的に何に気を付ければよいのか」という疑問があると思います。

![](https://assets.st-note.com/img/1738650970-8oTyZa7JNs5DmFBpWMqC0gtI.png?width=1200)

SRE（Site Reliability Engineering）の文脈になってしまいますが、一般的にサービスが継続して機能するためには開発者がSLI（Service Level Indicator）やSLO（Service Level Objective）に沿った監視を行い、サービスの健全性を保証する状態を保つことが理想とされています。  
これはよくあるピラミッドですが、一番ベースになるものは「モニタリング」です。

![](https://assets.st-note.com/img/1738650989-0OBHQi1IamuK4D3wxT9LAbpS.png?width=1200)

モニタリングとは、メトリクスやトレースといった指標を収集し、システムの状態を把握できるようにするものです。

![](https://assets.st-note.com/img/1738651011-Yu3vyxn4WteNCJAOp72rq0HD.png?width=1200)

たとえば、監視対象となる指標として「APIごとのリクエスト数」、「CPUやメモリの使用率」、「APIごとのエラー率」などがあります。これらのメトリクスをどのように監視するか、また、DatadogやNew Relicといった監視ツールをどのように組み合わせるかが重要です。  
また、テレメトリデータが必要とされる場合には、アプリケーションに適切な実装を追加する必要があります。さらに、エラーハンドリングをして、それをプロバイダ側に送信する仕組みを整えなければいけません。

![](https://assets.st-note.com/img/1738651031-6tl5G3jHO4rzeLFfJcwkCmoU.png?width=1200)

このアプリケーション部分をHonoやTypeScriptでどうするかをお話します。

![](https://assets.st-note.com/img/1738651057-L5mERp8gO7tXcZNaUGATVhK1.png?width=1200)

Honoにはエラーハンドリングの仕組みが用意されており、トップレベルのOnErrorメソッドを使用してすべてのエラーをキャッチすることが可能です。

![](https://assets.st-note.com/img/1738651133-JhGUSMkeRViEXDWZ54IQd0Pv.png?width=1200)

たとえば、認証エラーやHTTPステータスコードを指定した例外をスローすることで、Hono側でパスできるようになっています。

![](https://assets.st-note.com/img/1738651152-aQCVJ4bEARqHitIu8XshFpU7.png?width=1200)

次に、構造化ロギングについて説明します。  
Loggerなどいろいろ扱う際にNode.jsの環境だとリクエストが重ならないような工夫が必要になりますが、HonoではContextがリクエスト単位で生成されるオブジェクトになっているので基本的にはこれをベースにリクエストごとに一意に扱いたい値を伝搬させていく方針になります。

![](https://assets.st-note.com/img/1738651171-i243MQXD6uTINOZHvjp5BAGc.png?width=1200)

具体的には、画像のように値をセットし、

![](https://assets.st-note.com/img/1738651196-SPf5W213G6uVcXt0QhxeAiOU.png?width=1200)

ミドルウェアに挟み込みます。  
Honoにはビルトインのミドルウェアが用意されており、グローバルでContextを扱える仕組みが備わっています。

![](https://assets.st-note.com/img/1738651218-cwdSFqKX9B2Gl1UNRgyjpaOz.png?width=1200)

これを利用することで、任意のタイミングでContextを取り出し、その中からLoggerなど必要なものを取得可能になります。

![](https://assets.st-note.com/img/1738651239-I24fDbz5FnlQSXsE1dc8CyrG.png?width=1200)

メトリクスとトレースの処理も基本的には同様です。ベンダーごとに仕様の違いはあるものの、近年ではOpenTelemetryのようなベンダーフリーなフレームワークが主流です。これを組み合わせることで、効率的にメトリクスを取得できます。

![](https://assets.st-note.com/img/1738651258-A8Nm1jSEDY4PMbpZi9fdVGh5.png?width=1200)

ロガー同様にこれもContextで伝搬させればよいです。Context内に保持するオブジェクトに型付けをして

![](https://assets.st-note.com/img/1738651276-W8bp2AENGKSogYcXuvDM1kni.png?width=1200)

そこから取り出して、

![](https://assets.st-note.com/img/1738651293-E3NOYXVHDTCkFBvqIQ8ZxmcA.png?width=1200)

ビジネスロジックに計装をします。

![](https://assets.st-note.com/img/1738651310-BvmlWh4YHXEN8rx590Q3UsPC.png?width=1200)

これを行うことで最終的にこのようにいろいろとトレースできて内部状態を見られるようになります。

### まとめ

![](https://assets.st-note.com/img/1738651337-RYF7z6gbECeAGlN5hyuJTmfp.png?width=1200)

「Hono×TypeScriptで最高の開発体験を得よう」ということで、私からの発表を終わらせていただきます。

**大谷：**石井さん、ありがとうございました。  
Xからも「SRE目線でHonoを語っていただけて助かった」との感想が寄せられています。それでは、続いて伊藤さんのLTに移ります。

[#2 へ続く](https://note.com/offers_deepdive/n/na8060e6488ba)