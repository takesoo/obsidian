---
AI summary: Notionのツリー構造の深化は情報のサイロ化を引き起こすことがあるが、Wiki機能を使えばデータベースのような機能を持ちながら、Wikiのように情報を閲覧できる。WikiデータベースはNotionの可視性を高め、社内ポータル等を運用するのに便利な機能をたくさん備えていると感じられる。
Created: Invalid date
URL: https://blog.cloudnative.co.jp/15990/
カテゴリー: Notionの使い方
---
[![](https://blog.cloudnative.co.jp/wp-content/uploads/2023/04/fda16b3cc8eb0e88485a6c0ee80ddc52.jpg)](https://blog.cloudnative.co.jp/wp-content/uploads/2023/04/fda16b3cc8eb0e88485a6c0ee80ddc52.jpg)

---

セキュリティチームのぐっちーです。社内Wiki等の用途で利用されているNotionが、新しく「Wiki機能」の提供を開始しました。（ややこしいｗ）以前から社内Wikiとして利用されていたNotionですが、「Wiki機能」の登場により、その用途に対してより管理しやすくなった印象です。

**本ブログでの用語の定義**

本ブログでは、以下の定義で文章を記載しています。WikiとWiki機能などは混同しないように注意してください。

- Wiki：不特定多数のユーザーが共同してウェブブラウザから直接コンテンツを編集するシステム、またはWebサイト。世間一般的な意味で使ってます。（Wikipediaより引用[1]）
- Wiki機能・Wikiデータベース：本ブログで紹介しているNotion社が新しく提供を開始した機能のことです。
- Notionページ：Notionの最も基本的な単位。文章を書いたり、表を挿入したりと様々な用途で利用できる。（[参考](https://www.notion.so/ja-jp/help/create-your-first-page)）
- Notionデータベース：ページの集合体。データベースにすることで検索がしやすくなったり、タグをつけて情報を整理しやすくすることができます。（[参考](https://www.notion.so/ja-jp/help/intro-to-databases)）

## Notionはただのドキュメント置き場にあらず

Notionは、単なるドキュメント置き場にとどまらず、情報の透明性とコラボレーションを促進できるツールだと考えています。メモ、文書作成、タスク、工程表、社内Wiki、データベースなどの機能を持ち、社内で乱立しがちなツールを統合することで、様々な場所に分散する情報を整理整頓することができます。また、Notionはリアルタイムでの編集機能やコメント機能を備えており、チームメンバー間や、別チームや社外に対しての情報共有が容易になります。

このようにNotionは企業やチームが直面する情報管理やコラボレーションの課題を解決し、情報の透明性の強化と、コラボレーションを加速させる役割を果たしています。

## ツリー構造の深化に伴う情報のサイロ化問題

そんなNotionを運用する上で悩ましいのは「ツリー構造の深化」です。Notionのツリー構造の深化は、情報のサイロ化を引き起こすことがあります。ツリー構造が複雑になると、チームメンバーが必要な情報にアクセスすることが困難になり、情報が局所化されてしまうことがあります。下記の例のように、Notionをたくさんのメンバーで自由に使っていると次第にツリー構造が深くなってしまう現場が往々にしてあり、そのような現場ではせっかく作った情報が活用される機会が減ってしまいます。

この図はNotionのMermaidという機能で作りました♪

また、異なる部署やプロジェクト間で情報が共有されず、知識の断片化が生じることが懸念されます。このような状況は、チームの生産性や効率性を低下させる原因となり、組織全体の情報管理やコラボレーションに悪影響を与えることがあります。

## データベース＋ページメンション or リンクドデータベース運用の辛さ

一方で、情報をツリー上に配置して、UIで辿れるようにしたいというニーズが少なからずあることも否めません。そのような要件下で情報はなるべく1つの場所に集約したい場合は、「Notionのデータベース」にドキュメントを集約し、「Notionのデータベース」上のページをページメンションするアプローチがあります。これをすることで、「Notionのデータベース」上にデータを集約しつつ、ユーザーから見るとツリーに近い形で情報を閲覧することが可能です。

また、「リンクドデータベース[2]」という機能を使い、データベースをフィルタリングした上で、複数の場所（同じページ内やワークスペース内の他の場所）で表示するようなアプローチがあります。タグなどをつけておくと、他の場所でソートして表示するのが比較的簡単になります。

しかし、いずれのアプローチもNotionを使うユーザーへの教育と手間があって成り立つモノであり、やや運用が大変という側面があります。教育が行き届かなかった結果、運用が破綻することも少なくありません。

## 新しく生まれた「Wiki機能」をつかってみる

**ご注意**

本ブログの内容は、2023年4月10日時点までの情報を元に作成しておりますが、クラウドサービスの仕様変更等に伴い、将来的に状況が変化することがございます。当社側で仕様変更が確認できた場合は可能な限り修正をしますが、最新の情報を常に維持することは難しい点についてはご了承ください。

そんな中、登場したのが、Notionの「Wiki機能」です。この「Wiki機能」は従来の「Notionデータベース」とは異なり、Notionを社内Wikiとして利用する場合に役立つ機能を備えたものです。Wikiでありながらデータベースのような機能をもつことから本ブログでは、「Wikiデータベース」と呼びたいと思います。利用方法はNotionページをWikiに変換する方法は右上にある [•••]メニューをクリックし、[Wikiに変換] を選択するだけです。（データベース配下のページなど、変換できないケースもあります）

## Wikiデータベース配下の「ページ」を一覧表示する

「Wikiデータベース」を作ったら、次は「ビュー」について説明します。同じページでも、様々な型式でページを見ることができます。「Wikiデータベース」には3つのデフォルトビューがあり、さらにカレンダー、タイムライン、リストなど他のレイアウトのビューを追加で作成することも可能です。

### **[ホーム**] ビュー

「ホーム」はデフォルトの表示画面です。「Notionページ」と同じような利用感で、様々なコンテンツを配置しカスタマイズすることができます。下記の図では、チーム別に編集するWikiのトップページを作ってみました。

### [**すべてのページ**] ビュー

特筆すべきは [すべてのページ] です。このビューを使うとWikiデータベースの配下のページを全て一覧で見ることができます。冒頭で紹介したような階層が深くなったページであっても、比較的用意に発見したり、オーナー（管理者）を見つけることができます。下記の参考画像のWikiデータベースには、4つの階層に渡ってページを設定していますが、全てのページが一覧で見ることができました。

**注意**

「Wikiデータベース」の配下に「Notionデータベース」を設置しても、「Notionデータベース」内のページは [すべてのページ] のビューには表示されませんでした。

### [**自分が所有するページ**] ビュー

[すべてのページ] と似ていますが、[自分が所有するページ]は自分がオーナーであるページのみを表示するデータベースビューです。自分がメンテナンスすべきドキュメントを一発で見つけることができます。

## Wikiデータベース配下を検索する

これまでも、Notionでは「チームスペース」単位や、「Notionデータベース」単位、「Notionページ」単位で絞って検索を行うことができました。「Wikiデータベース」に変換した「Notionページ」も、通常の「Notionページ」と同様に、配下のドキュメントを検索することができるので、ここで絞ることで検索性が上がることが期待されます。

## プロパティは「Wikiデータベース」全体に引き継がれる

Notionデータベースにはプロパティ[3]という概念があり、期限やタスクの担当者、関連URL、最終更新日時など、あらゆる種類のコンテキストを与えることができます。この機能はNotionデータベース内のページ全体に共通して何かしらの情報を付与したり、検索条件として利用することができます。

「Wikiデータベース」にもプロパティの概念がありますが、「Wikiデータベース」のプロパティを編集すると「Wikiデータベース」全体に適用され、上位ページや下位のページに引き継がれるというのがNotionデータベースと異なる点です。下記ページにも引き継がれるプロパティを利用して、上位ページから下位ページまでを横断検索したりすることができそうなので、非常に便利だと思いました。

## Wiki機能 on Wiki機能 はできない

巨大組織でNotionを利用したい際にWikiデータベースの配下にWikiデータベースを設置するというユースケースがあるのでは？と考えました。しかし現時点では、Wikiデータベースの配下には、Wikiデータベースを設置できない（変換・移動ができない）という結果でした。

## ページオーナーと有効期限を表示する

「Wiki機能」において、新しく追加された概念として「オーナー」と「有効期限」があります。まず、「オーナー」ですが、文字通りページを責任もってメンテナンスする人物を指定することができます。「オーナー」はデフォルトではNotionページを作成した人物がアサインされますが、変更することができます。

続いて、「有効期限」ですが、これはページの有効期限を1日から無制限の期間を指定して設定することができます。有効期限期間中は、そのページに対して「有効」というマークがつくので、他のユーザーに対してページ内のコンテンツの賞味期限を示すことができます。さらには、有効期限が切れたタイミングで「オーナー」に対して有効期限が切れたことを通知してくれます。有効期限が切れたタイミングでコンテンツの中身自体が変更されることはありませんが、「オーナー」はNotionからの通知を契機にコンテンツの見直しを行うことができます。

「有効期限」の利用については、ユースケースに次第ですが、ざっと以下のようなユースケースが考えられます。会社やページごとにポリシーを決めて運用するといいと思いました。

- 総務系：「健康診断のお知らせ」について、健康診断の終了日を有効期限に指定する。
- 人事系：「2023年度新卒採用リファラル制度」のページで、採用の終了日を有効期限に指定する。
- エンジニア系：「製品検証メモ」の有効期限を1年に指定する。（製品のアップデートにより変わっている可能性が高いため）
- その他：「外部公開ページ」の有効期限を6ヶ月で指定して、都度アクセス権の見直しを行う。

**参考**

Wikiデータベースごとに、デフォルトの有効期限を指定できたら嬉しいなと思いましたが、現時点ではそのような機能はないようです。

## おわりに

今回登場した「Wikiデータベース」はその名の通り、Notionの可視性を高め、社内ポータル等を運用するのに便利な機能をたくさん備えていると感じました。特に[すべてのページ]ビューでは、階層が深くなったドキュメントを簡単に発見できるのがとても良いと思いました。読者の皆様も、試していただけると嬉しいです。

### 参考文献

- Wikiと有効性確認済みページ [https://www.notion.so/ja-jp/help/wikis-and-verified-pages](https://www.notion.so/ja-jp/help/wikis-and-verified-pages)
- Notion [https://cloudnative.co.jp/product/Notion](https://cloudnative.co.jp/product/Notion)

### 注釈

1. ウィキ [https://ja.wikipedia.org/wiki/ウィキ](https://ja.wikipedia.org/wiki/%E3%82%A6%E3%82%A3%E3%82%AD)
2. データソースとリンクドデータベース [https://www.notion.so/ja-jp/help/data-sources-and-linked-databases](https://www.notion.so/ja-jp/help/data-sources-and-linked-databases)
3. データベースのプロパティ [https://www.notion.so/ja-jp/help/database-properties](https://www.notion.so/ja-jp/help/database-properties)