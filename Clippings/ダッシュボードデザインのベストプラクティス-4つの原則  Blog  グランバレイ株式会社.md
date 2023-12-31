---
Created: Invalid date
URL: https://www.granvalley.co.jp/blog/4-design-principles-creating-better-dashboards
---
[![](https://www.granvalley.co.jp/wp/wp-content/uploads/2018/07/header_dashboard-mockup_of_sisense.jpg)](https://www.granvalley.co.jp/wp/wp-content/uploads/2018/07/header_dashboard-mockup_of_sisense.jpg)

---

旧来の[BIツール](https://www.granvalley.co.jp/analytics/)は、Excelライクなユーザーインターフェースが採用されていましたが、モバイルファーストの今、BIツールのユーザーインターフェースは、モダンで視野性と操作性が高いデザインが採用されてきています。

日常的に業務で最も使われる「ダッシュボード」。今回、このダッシュボード設計のベストプラクティスをお届けします。

## 優れたダッシュボードの設計方法とは

・**複雑なものを単純化**：大量の情報、常に変化する多くのデータ、さまざまな分析ニーズに対する問いがあります。この複雑さをすべて克服して簡単にする必要があります。  
・**問いに答えられるレイアウト**：データをビジネスのコンテキストに結びつけ、利用者の問いに答えることができるようにする必要があります。ダッシュボードの視覚的レイアウトがこの解決に重要な役割を果たします。  
・**データの意味を表現**：選択されたデータを適切に視覚化するには、データとそれから抽出する情報を正しく表現する必要があります。  
・**適切な明細を表示**：利用者により概要だけではなく、より深堀するために明細を表示する必要があります。そのためには、利用者が必要なデータに常にアクセスできるようにします。

業務ごとのダッシュボードには、独自の要件、制限事項、およびKPIがあります。本来、ダッシュボードを作成するにあたり定説があります。  
今回、数ある定説の中、4つの主要原則にフォーカスをして、「最適なダッシュボードとは何か？」を紐解きます。

## 利用しにくいダッシュボードとは

最初に、設計が不適切で利用しにくいダッシュボードとはどのようなものか検証します。

このダッシュボードのデザインは、どこが悪いと思いますか？

・多すぎるウィジェット（約30個）、ビジュアル化とインジケータによる視覚的な混乱してしまう  
・「売り上げ総額は？」などの基本的な問いに対して、答えを得るのに5秒以上の時間がかかってしまう  
・どの組織に課題があるのかわからない、ヒエラルキーを意識したデザインではない  
・洞察の仕方や傾向がわからない

## 4つのベストプラクティス

最適なダッシュボードの設計原則とは何でしょうか？以下にその原則を記載しました。

### 5秒ルール

ダッシュボードとは、閲覧者に対し約5秒で関連情報を提供する必要があります。

つまり、情報を理解するのに数分かかるのは、ダッシュボードのビジュアルレイアウトに問題がある可能性があります。

ダッシュボードを設計するときは、5秒ルールに従うようにしてください。

これは、ダッシュボードを検討しているときに、必要な情報を見つけるために必要な時間です。  
もちろん、臨時調査には明らかに時間がかかります。彼女の勤務中にダッシュボードユーザーに最も頻繁に必要とされる最も重要な指標は、すぐに画面から「ポップ」する必要があります。

### 論理レイアウト：逆ピラミッド型

ダッシュボードを設計するときは、何らかの整理の原則に従うことが重要です。

最も重要な「指標」はダッシュ​​ボードの上部に、そして、中間は「傾向」を、下部には「詳細」を表示します。

その原則のうち、最も有用な方法の1つが、逆ピラミッド型です。

このコンセプトは、ジャーナリズムの世界に由来し、基本的には、ニュースレポートの内容を3つに分けて、重要性が減る順番で示しています。  
最も注目するべき情報が一番上に表示され、中盤はより重要な詳細な情報が含まれ、下部には読者や視聴者がより深く知ることができる一般的な情報があります  
（ニュース記事の見出し、小見出し、本文を考えてください）。

### ミニマリズム：もっと少なく

各ダッシュボードには、5〜9個の表やグラフを含める必要があります。

ダッシュボードの設計者の一部には、とにかく数多くの表やグラフを入れたダッシュ​​ボードを提供したいと思っている人もいます。  
これは理論的には良いと言えるかもしれませんが、残念ですが、認知心理学では人間の脳は一度に約7 + 2しか理解できないため、多くの情報を提供すると人は混乱してしまいます。  
そのため、これがダッシュボードに必要かつ適切なアイテム量です。

しかしながら、より多くの情報を見せる必要が出てきます。その場合は、フィルタと階層を使用してデータを階層化することは、視覚的な混乱を避けることができます  
（たとえば、北アメリカと南米の売上量を表す指標の代わりに、同じインジケータを変更するフィルタを適用するオプションをユーザに与えるもう1つ）。  
ほかの方法では単にダッシュボードを2つ以上に分割してダッシュボードを複数にすることも必要です。

### 適切なデータの視覚化の選択

データビジュアライゼーションを適切に使えれば、基本的な表形式よりも効果的であり具体的な目的に役立ちます。

ビジュアライゼーションを選択する前に、情報の関係性を検討する必要があります。

- **関係** – 2つ以上の変数間を接続してみる
- **比較** – 2つ以上の変数を並べて比較してみる
- **合成** – データを別々のコンポーネントに分割してみる
- **分布** – データ内の値の範囲を決めグループ化してみる

もし戸惑った場合は、インタラクティブウィザードを使用して、適切なデータビジュアライゼーションを選択してください

## ダッシュボードの設計：何を考慮するべきか

利用者が見ているものを理解できるようにするためには、適切な表やグラフを選択することが鍵となります。しかしながら、それだけではありません。  
ダッシュボードを設計する方法を考える際、最初に誰がこのダッシュボードを使うのか考える必要があります。

たとえば、広告プラットフォームの最適化に重点を置いたダッシュボードを設計するときは、ウィジェットを指標に当ててコンバージョン率を高めることが望ましいでしょう。

例えば、オンラインマーケティング担当者は、日々のあらゆるレベルであらゆる広告を配信し続けるために、CPM（インプレッション単価）をKPIとしてみたいでしょう。  
しかし、マーケティングの責任者からすると、広告の掲載結果がどのように変化してリードがもたらされたかなど、広い視野でで結果を見たいと思うかもしれません。

そのために、まずダッシュボード設計に入る前に、利用者を特定し一緒に要件を収集し、KPIを定義することをしてください。。  
もちろん、このようなことをしなくても美しいダッシュボードを設計できますが、しかしながら、意思決定を支援できず、結果使われないダッシュボードになってしまいます。

本記事は、[Sisense](https://www.granvalley.co.jp/analytics/products-sisense)社の許諾のもと弊社独自で記事化しました。

[https://www.sisense.com/blog/4-design-principles-creating-better-dashboards/](https://www.sisense.com/blog/4-design-principles-creating-better-dashboards/)

※ [Sisense](https://www.granvalley.co.jp/analytics/products-sisense) は、Sisense Inc の商標または登録商標です。  
※ その他の会社名、製品名は各社の登録商標または商標です。  
※ 記事の内容は記事公開時点での情報です。閲覧頂いた時点では異なる可能性がございます。