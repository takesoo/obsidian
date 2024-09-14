---
finished reading: true
---
# 分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた #DataEngineeringStudy | DevelopersIO
  #ReadItLater 
 #ReadableArticle

## articleURL
https://dev.classmethod.jp/articles/report-on-designing-data-intensive-applications/

## siteName
分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた #DataEngineeringStudy | DevelopersIO

## date
2024-09-13

## articleContent
[Martin Kleppmann](https://martin.kleppmann.com/)著の『データ指向アプリケーションデザイン』をご存知でしょうか。 副題は「信頼性、拡張性、保守性の高い分散システム設計の原理」となっており、分散データシステム設計のあらゆるトピックを660ページに渡って網羅する百科事典のような書籍です。

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/book-cover-data-intensive-application-design-320x408.jpg)](https://www.amazon.co.jp/dp/4873118700)

[『データ指向アプリケーションデザイン ―信頼性、拡張性、保守性の高い分散システム設計の原理』 by Martin Kleppmann](https://www.amazon.co.jp/dp/4873118700)

私自身は、現職でAWSリセールのビリングシステムのデータエンジニアリングに数年間ガッツリと従事していた時期があり、本書の読書会を社内([Notoさん](https://dev.classmethod.jp/author/noto-satoshi/)&[石川さん](https://dev.classmethod.jp/author/ishikawa-satoru/))・社外の有志と2.5年、28回かけて昨年無事完遂しました。

思い入れ深い本書を、監訳者の斉藤太郎氏がデータエンジニアリングの切り口でわずか30分にまとめてくれるイベントが開催されるということで、楽しみながら講演を聴講しました。

[Data Engineering Study #18「データ指向アプリケーションデザイン」 - connpass](https://forkwell.connpass.com/event/269125/)

これから分散データベースに入門したい人、読んだけど難しくて挫折した人、復習したい人、多くの人にとって学びの多い講演でした。

個人的な感想は、[@t\_wada](https://twitter.com/t_wada) さんの次のツィートに集約されています(笑)

> 週1回のペースで14ヶ月かけて読んだ（自分の場合）大著『データ指向アプリケーションデザイン』の概要を30分でつかめる最高に「タイパ」の良い資料だ / “30分でわかるデータ指向アプリケーションデザイン - Data Engineering Study #18” [https://t.co/TGXdY2H8Y8](https://t.co/TGXdY2H8Y8)
> 
> — Takuto Wada (@t\_wada) [February 15, 2023](https://twitter.com/t_wada/status/1626004466616127488?ref_src=twsrc%5Etfw)

あれだけ膨大な知識が詰め込まれた重厚な技術書をたった30分でわかった気になれるって、これ以上の**タイパ**はなかなか得られないと思います。

今回のイベント「Data Engineering Study」を共催したInfra Study Meetup を運営する Forkwell社と、分析基盤向けデータ統合SaaS「trocco」の開発・運営を行う primeNumber社に感謝です。

本レポートでは基調講演を中心にイベント内容を共有します。

本書を未読の場合、監訳者によるまえがきも一読することもおすすめします。

[データ指向アプリケーションデザイン. 信頼性、拡張性、保守性の高い分散システム設計の原理 | by Taro L. Saito | Medium](https://medium.com/@taroleo/ddia-63a454e44dc9)

## イベント概要

-   Data Engineering Study #18「データ指向アプリケーションデザイン」
-   2023/02/15 開催

こんなエンジニアにおすすめ

-   データ指向アプリケーションデザインを読んでいる・読もうと思っている方
-   ソフトウェアエンジニア・アーキテクトの方
-   これからデータエンジニアを目指す方

タイムスケジュール

| 時間 | 内容 | 発表者 |
| --- | --- | --- |
| 14:00〜 | オープニング（10分） |  |
| 14:10〜 | 基調講演「30分でわかるデータ指向アプリケーションデザイン」（30分） | Principal Software Engineer , Treasure Data 斉藤 太郎 氏 |
| 14:40〜 | 休憩 / スポンサーLT（5分） | Forkwell |
| 14:45〜 | 質疑応答（20分） |  |
| 15:05〜 | スポンサーLT「troccoフリープランはじめてみた」（5分） | primeNumber 小林 寛和 氏 |
| 15:10〜 | トークセッション（50分）   ※勉強会のテーマ上、データ分析基盤周りの話を中心にします   \- 本書籍を更に深く学ぶ方法   \- 本書籍からの技術進歩に関するアップデートについて   \- 本書籍の技術の応用について   \- 本書籍を特に読むべき人・タイミングなど | パネリスト: 田籠 聡 氏 / 斉藤 太郎 氏   モデレーター: primeNumber 小林 寛和 氏 |
| 16:00〜 | アフタートーク（10分） |  |
| 16:30 | 完全終了 |  |

## 基調講演「30分でわかるデータ指向アプリケーションデザイン」

・ スピーカー

斉藤 太郎氏 　[Twitter：@taroleo](https://twitter.com/taroleo) / Github：@xerial

Principal Software Engineer , Treasure Data

東京大学理学部情報科学科卒。情報理工学 Ph.D。データベース、大規模ゲノムデータ処理の研究に従事。その後、スタートアップであるTreasure Dataに加わり、アメリカ、シリコンバレーを拠点に活動中。日本データベース学会上林奨励賞受賞。OSSを中心にプログラミングやデータ処理を簡単にするためのプロダクトを作成している。

> 「30分でわかるデータ指向アプリケーションデザイン」最新の論文にも触れながら、分散データシステムの世界の魅力を伝えていきます。後半、[@tagomoris](https://twitter.com/tagomoris?ref_src=twsrc%5Etfw) [https://t.co/TQ2TnsFIOT](https://t.co/TQ2TnsFIOT)…
> 
> — Taro L. Saito (@taroleo) [February 16, 2023](https://twitter.com/taroleo/status/1626099819914858496?ref_src=twsrc%5Etfw)

・ 発表スライド

### 「データ指向」とは?

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-glossary-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-glossary.jpg)

原著のタイトルは"Designing Data-intensive Applications"。 「データ指向」は本書のために生み出された訳語。

"data intensive"の訳語が難しい。"CPU intensive"や"memory intensive"にならうと"data intensive"の訳語は「データ集中」や「データ特化型」になるが、しっくりこない。オブジェクトを中心にプログラムを設計する"Object Orinted"は「オブジェクト指向」と訳されている。 これにならって「データ指向」とした。

本書では、データを様々な角度から扱っており、データの量、複雑さ、変化の速度などを中心に考えるデザインのことを**「データ指向アプリケーションデザイン」**と定義している。

原著が出版されたのは2017年で、翻訳本は2年後の2019年。出版から5年も経過し、新しい技術、論文、製品が次々と生み出されている。 本イベントでは、本書でカバーできていない、発展的なトピックを"Advanced Topic"として紹介。

これらは「データ指向アプリケーションデザイン」に書かれている基礎の延長上にある。 新しいトピックがあるからと言って、5年前に出版された本書を読むのが無駄になっていないのが、本書のいいところ。

データの量・複雑さ・変化の速度に対応できるシステム設計をどうすればいいのか?

### データの表現：データ量が少ない場合

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-small-data-format-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-small-data-format.jpg)

#### テキストデータフォーマット

データ量が少なければ、表現・生成しやすさすいテキストデータフォーマットがよく使われる。

-   CSV
-   JSON
-   XML

などがよく使われる。

ただし、テキストなので、浮動小数点やバイナリデータの扱いが不便。

#### バイナリデータフォーマット

そのためバイナリデータフォーマットが使われる

-   MessagePack
-   Trhift
-   ProtocolBuffers
-   Apache Avro([著者のMartin KleppmannはAvroのコミッター](https://avro.apache.org/project/credits/))

これらテキスト/バイナリデータフォーマットは、データとして完結している(=データ自体にスキーマを持っている)自己記述型データフォーマット(self-describing)に分類される。

### データの表現：データ量が多い場合

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-big-data-format-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-big-data-format.jpg)

データ量が多くなってくると、特にデータ基盤では、省メモリ、圧縮しやすさ、ストレージへの格納しやすさが重視されてくる。

スキーマと実際のレコードを分離することで、データをコンパクトに表現できる。

#### 行指向(row-oriented)フォーマット

-   RDBMSでよく使用
-   テーブルの横方向にデータを分解して管理
-   1レコードずつ表現するので、レコード単位の更新が簡単

#### 列指向(column-oriented)フォーマット

-   最近のデファクトスタンダード
-   テーブルの縦方向にデータを分解して管理
-   なぜ縦方向に分解するかというと、同じ型のデータを集めておくと、非常にコンパクトに圧縮できる
-   キャッシュにも乗りやく、SIMD演算も使えるので、分析クエリに特化したデータフォーマット
-   1レコードを複数のカラムブロックで表現するので、更新のオーバーヘッドが高い

### 列志向データフォーマット(Columnar Format)

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-columnar-format-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-columnar-format.jpg)

オープンフォーマットとしてはApache Parquet(「「パーケ」と発音)がよく利用される。

"Parquet"はフランス語で

-   木目の床模様
-   木目の床

を表し([Google イメージ検索](https://www.google.com/search?q=Parquet&tbm=isch))、カラム表現にあった言い方。

Google Dremel(VLDB 2010)のアイデアがベースになっている。

[Dremel: Interactive Analysis of Web-Scale Datasets – Google Research](https://research.google/pubs/pub36632/)

ネストして繰り返しのあるデータでも、同じパスにあるデータだけを取り出して、縦方向に圧縮できる。

ネストしたデータを表すように、ツリー構造用のエンコーディングも入っていて、 クラウド上のストレージ(Amazon S3など)で扱いやすくするために、ページブロックに分けたり、フッターに、ページへのインデックスをつけて圧縮して保存すると言った工夫がなされている。

最近では、Sparkだけでなく、DuckDBなどいろいろなデータベースで採用されている。

Amazonでも、Parquetを採用したサービスがどんどん出てきていて、非常によく使われるフォーマット。

### 変化しない(immutable)データ

データはどのくらい変化するのか?

ログのようなあまり変化しないイミュータブルなデータの場合、データを列志向に保存し、ストレージサイズを節約したり、分析クエリを速くしたりする。こういうのを**読み込み特化型(read-intensive)**と呼び、データへの高速なアクセス、分析の速さが問われる。

分析クエリを速くする常套手段に**データの分散**がある。

#### パーティショニング(シャーディング)

データを

-   時間範囲
-   キー

などで分割して別々の領域に保存する。

-   必要な部分のデータにだけアクセス
-   別領域のデータへ並列アクセス

#### レプリケーション

データ自体のコピーをあちこちのノードにおいて、複数台のノードで同時に別々のディスクを読めるようにして高速化する。

バックアップ用途にレプリケーションすることもある。

#### 実体化ビュー(マテリアライズド・ビュー)

クエリー結果をファイルに保存しておき、再利用する。

### 変化する(mutable)データ

書き込みが沢山ある場合は、どうすればよいか？

RDBMSが歴史的に非常に強い分野で、レコード単位の更新だったり、トランザクション処理が必要な場合に、こういうデザインが採用される。

書き込みに特化すると言っても、書き込みだけが速ければいいわけではない。 編集したいデータがどこにあるか素早く見つけないといけないので、書き込みを速くするけれども、同時に、検索も速くしないといけないところが難しい。

そのためのインデックス構造がデータ指向アプリケーションデザインでいくつか紹介されている

-   B-Tree
-   Hash index
-   SSTable(Sorted String Table)

### B-Tree インデックス

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-b-tree-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-b-tree.jpg)

B-Treeは非常に優秀な構造。 ディスク上のページに、次のページへのルックアップテーブルを格納する(ポインター管理)。

RDBMSは`CREATE INDEX`命令を実行すると、対応するB-Treeインデックスが作成される。 検索を速くしたいからと言って、たくさんインデックスを作ると、1個レコードを更新するたびに複数のB-treeインデックスを更新しないといけないので、注意が必要。

B-Treeは上手に使うと、高い更新性能を発揮する。 空き領域にどんどんデータを挿入できるし、ページ数が増えても、木の高さがあまり高くならないので、ディスクへのランダムアクセス回数を減らすのに役立つ。

各ノードに100個のエントリーが含まれている場合、1億個のレコードをB-Treeに追加しても、木の高さは `log_100(1億)＝ log_100(100 ^ 4) = 4` の深さにしかならない。ナイーブにやると、1億レコードをフルスキャンしないといけないのに、B-Treeインデックスを利用すると、4回ディスクをルックアップすれば、1億件のエントリーから目的のデータを見つけられるという意味で、非常に優秀なデータ構造。

### Log-Structureed Merge(LSM) Tree インデックス

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-lsm-tree-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-lsm-tree.jpg)

B-Treeインデックスをさらに書き込みに強くしたデータ構造として、Log-Structureed Merge(LSM) Treeが最近よく使われている。

レコードを追加する操作と、バックグラウンドでレコードをきれいに整形してマージする操作(コンパクション)を分離している。

メモリにどんどんデータを追加し、ランダムアクセスに強いメモリ上でデータを並び替える。 バックグラウンドでソート済みの異なるレイヤー間のデータをマージソートし、次のレイヤーに保存する。

ソート処理はランダムアクセスに強いメモリ上で行い、バックグラウンドでソート済み データを書き出す際はシーケンシャルアクセスなので、非常に優秀。

SSDの中身も似たような構造が使われている。 SSDにはバッテリー付きのキャッシュメモリーが入っていることが多くて、そこでランダムアクセスを吸収してから、 SSDのメモリセルにシーケンシャルにアクセスし、セルの寿命を延ばす工夫もされていたりする。

LSM Treeは面白いけれども、読み込み時に各レイヤーにアクセスしないといけないので、その分のオーバーヘッドがある。 書き込み時も、B-Tree インデックスは1回書き込めばよいけど、LSM Treeインデックスはデータをどんどん次のレイヤーに書き出していくので、同じデータの書き込みが何回も生じる。これを**書き込みの増幅(Write Amplification)**と呼び、意外とディスクのライトが多くなる。

-   LevelDB
    -   Google。Google BigTableにインスパイアされ、2004年のBigTable論文と同じ著者が実装
    -   C++
-   RocksDB
    -   Facebook
    -   C++
-   S3
    -   Amazon
    -   Rust
    -   SOSP 21 Best Paper Award:[Using lightweight formal methods to validate a key-value storage node in Amazon S3 - Amazon Science](https://www.amazon.science/publications/using-lightweight-formal-methods-to-validate-a-key-value-storage-node-in-amazon-s3)

などでLSM Treeが使われている。

### データが複数のノードにまたがるとどうなる?

インデックスを複数のノードにデータをもたせたら、どういう難しさがあるか？

データが分散することで問題の複雑さが数段上がる。

#### 分散システムが必須

ノード間でデータをやりとりするので、分散システムを作らないといけない

-   各ノードがステートマシンを動かす:サーバー間の状態を同じ状態にするため
-   サーバー・クライアント間でのデータ通信(REST/RPC)

#### 複数ノードで頻繁に更新される場合

-   同期(コンカレンシーコントロール＝MVCCのCC)
    -   ロックの取得、タイムアウト、トランザクション、ログのリプレイなどいろいろ考えないといけない
-   どの状態が正解かを決めるために、合意(コンセンサス)を取らないといけない
    -   基本的には多数決(quorum)で決め、そのためのアルゴリズムとして
        -   Paxos
        -   2PC(two Phase Commit)
        -   など
    -   ロックのような重い処理をさけるためにCoordination avoidanceなども使われている(Amazon Auroraなど)
        -   [Paper Summary: Coordination Avoidance in Database Systems](http://muratbuffalo.blogspot.com/2014/11/paper-summary-coordination-avoidance-in.html)

#### 障害対応

単純にデータを通信するだけではすまなくて、障害対応も大事

-   エラーがおきた時にどうするか
-   リクエストをリトライする時にどうするか
-   冪等性(idempotency):同じ操作を2回実行しても大丈夫なようにシステムを設計する
-   復旧をどうするか

もっと難しい問題として、ノードが嘘をつく(間違ったデータを返す)状態でもシステムが正しく動くように考えないといけない。 敵の中に裏切り者がいることから、[ビザンチン障害](https://www.nii.ac.jp/today/69/4.html)と呼ぶ

### 24時間365日動き続けるサービスの設計(SLA)

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-idempotency-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-idempotency.jpg)

#### 稼働率

指標になる数字として稼働率(availability)がある

稼働率ごとに許容されるダウンタイム(システムを止められる時間)は

-   99.9% : 8.7時間/年
-   99.99% : 53分/年
-   99.999 : 5分/年

#### リトライ処理

アプリケーションを1年同じ状態のまま動かし続けるのは不可能で、更新しないといけない。 アプリケーションをデプロイする時は、プログラムを一度止めて、次のバージョンのアップグレートしないといけないので、リクエストが一度止まる。

そのため、クライアント側ではリトライ処理を実装しないといけない。

サーバーがアップグレード中にリクエストが失敗しても、クライアントがリトライすることで、次のバージョンでちゃんとリクエスト処理される。

#### 冪等性

サーバーでは正しく処理されても、ネットワーク障害などにより、レスポンスがクライアントに届かないことがある。 クライアントがリトライすると、サーバーでは同じ処理が複数回行われる。 同じ操作が繰り返されても、1回として処理され、データが重複しないようにシステムを設計する。冪等性(idempotency)を保った設計。

リクエストにユニークなID(UUID)をつけたりする。

**エンジニア面接で冪等性について質問すると、エンジニアがどれくらい経験があるのかわかる良い指標になる。**

### ComputeとStorageの分離

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-compute-storage-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-compute-storage.jpg)

常にデプロイし続けられるように、コンピュート(クエリの実行)とストレージを分離するのが近年の標準的な設計になってきている。

こうすることで、クエリーエンジンとストレージを別々にスケールさせたり、ノードを一個ずつ(ローリング)アップグレードするのも可能。

-   Snowflake
    -   [The Snowflake Elastic Data Warehouse : SIGMOD 2016](https://www.snowflake.com/resource/sigmod-2016-paper-snowflake-elastic-data-warehouse/)
-   Amazon Redshift
    -   [Amazon Redshift re-invented : SIGMOD 2022](https://www.amazon.science/publications/amazon-redshift-re-invented)
    -   [論文から垣間見るAmazon Redshiftの進化と深化 2022 #jawsug #bdjaws | DevelopersIO](https://dev.classmethod.jp/articles/20221224-evolutions-of-amazon-redshift-from-paper/)

はコンピュートとストレージを完全に分離したデザインで、SIGMODの論文に詳しい。

Redshiftが面白いのは、Redshiftネイティブのストレージだけでなく、S3にあるデータもクエリーできるようデザインされており、コンピュートをGPUにして超並列クエリーを実行することもできる。

### トランザクション処理

次はデータを頻繁に更新する時に必要なトランザクション処理について。

**トランザクションはデータベースの花形技術。**

#### ACID はマーケティング用語

ACID(エイシッド)というキーワードを聞いたことがある人も多いハズ。

1983年頃に研究者が作ったキーワード。

-   Atomicity
-   Consitency
-   Isolation
-   Durability

ACID の定義は色々あるけれども、**単にACIDと使われる場合はほぼマーケティング用語と思ったほうがよい**。

例えば、何がどう保護されるのかは、consistencyがなにかとか、isolationになにをやっているのか、詳細を見ないと全くわからない。 むしろ、トランザクション分野に詳しくなればなるほど、分からなくなっていく。

> 【エンジニア用語解説】
> 
> 「完全に理解した」  
> 製品を利用をするためのチュートリアルを完了できたという意味。
> 
> 「なにもわからない」  
> 製品が本質的に抱える問題に直面するほど熟知が進んだという意味。
> 
> 「チョットデキル」  
> 同じ製品を自分でも１から作れるという意味。または開発者本人。
> 
> — 伊藤 祐策(パソコンの大先生) (@ito\_yusaku) [September 20, 2018](https://twitter.com/ito_yusaku/status/1042604780718157824?ref_src=twsrc%5Etfw)

このあたりを詳しく知りたければ、以下を読めばわかる

-   ["Transactional Information Systems: Theory, Algorithms, and the Practice of Concurrency Control and Recovery (The Morgan Kaufmann Series in Data Management Systems)" : Weikum, Gerhard, Vossen, Gottfried: Foreign Language Books](https://www.amazon.co.jp/dp/1558605088/)
-   ["Database Management Systems" : Ramakrishnan, Raghu, Gehrke, Johannes: Foreign Language Books](https://www.amazon.co.jp/dp/0071230572/)

本書でも、重要な概念は一通りさらってある。

#### 分離性(isolation)について

アイソレーション(分離)のいちばん重要な考え方は**直列化可能性(serializability)**。

直列化可能性はトランザクションが一つずつ実行されたのと変わらない状態を維持すること。 この性質を持たせるのがトランザクションの基本。 オーバーヘッドがかかるので、実際に使うべきかはよく考えないといけない。

そこで、[弱い分離性](http://www.redbook.io/ch6-isolation.html)というのが提案されてきていて

-   Read-Committed
-   Snapshot-Isolation
-   MVCC

というのもある。

分離性を妥協しないアプローチとして、スナップショットをとって性能を速くするけど、serializableなSerializable Snapshot Isolation(SSI)というテクニックもある(SIGMOD 2008 best paper)。

[Serializable isolation for snapshot databases | ACM Transactions on Database Systems](https://dl.acm.org/doi/abs/10.1145/1620585.1620587)

SSIが教科書にのらないかなぁと思っていたら、データ指向にのってすごく嬉しかった。

SSIはPostgreSQLなどで実装されて、皆さんが使うこともできる( [\*1](https://dev.classmethod.jp/articles/report-on-designing-data-intensive-applications/#note-1026407-1 "Serializable Snapshot Isolationは`SERIALIZABLE`、Snapshot Isolationは`REPEATABLE READ`に対応"))。

-   [https://wiki.postgresql.org/wiki/SSI](https://wiki.postgresql.org/wiki/SSI)
-   [https://wiki.postgresql.org/images/4/4f/SSI-PGConfEU2011.pdf](https://wiki.postgresql.org/images/4/4f/SSI-PGConfEU2011.pdf)

#### トランザクションでどういう異常(anomaly)がおこるか?

snapshot isolationで防げない異常

-   ファントム
    -   まだ存在しないレコードに、どうロックをかけるか
-   Write skew
    -   同じスナップショットを読んで、違う場所に書き込む

分離レベルの repeatable read は、 名前からは繰り返しデータを読めそうなトランザクションを想像するが、実はそんなことはない。

[Repeatable Read Is Not Repeatable | by Taro L. Saito | Database Journal Club | Medium](https://medium.com/db-journal/repeatable-read-is-not-repeatable-c700b2ce1c76)

この名前が出てきたら使うのはやめた方がいい、といったような説明が本書にある。

### 分散トランザクション

複数ノードでトランザクションを実現するには、いろいろな問題が出てくる。

複数のノード・システム間でトランザクションを実装するには一貫性、合意、耐障害性などを考えないといけない。

その時に重要な考え方が **線形化可能性(linearizability)**。

線形化可能性とは、データのコピー(レプリカ)が複数あっても、ユーザーにとっては一つしか無いように見せ、更新処理が終わったら、全ノードが同じデータを見られるように保証すること。

実装するには、いろいろな技術が必要

-   リーダー選出
-   サービスディスカバリ
-   メンバーシップ管理

これらが正しく実現できないと、1つのleaderノードに書き込みし、leaderはレプリカに変更イベントを送る、シングル・リーダー(leader)・レプリケーションなども正しく実装できない。

これら技術は

-   Zookeeper
-   etcd(Kubernetes内部で利用)

などでも実装されている。

最近のアプローチとして、日本から出てきた[ScalarDB](https://scalar-labs.com/)があり、アプリケーション側のクライアントで分散トランザクション処理を吸収し、データベースに依存しない。

[Scalar DBを用いた複数のデータベースを跨いだ分散トランザクション(Part 1) - Scalar Engineering (JA) - Medium](https://medium.com/scalar-engineering-ja/distributed-transactions-spanning-multiple-databases-with-scalar-db-part-1-bcb63734e554)

### Amazon Auroraは分散トランザクションの極み

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-aurora-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-aurora.jpg)

この辺の技術を本書で学ぶと、SIGMOND 2018で発表されたAmazon Auroraの技術がいかに洗練され、シンプルになっているかわかって面白い。

-   Amazon Auroraは分散版のMySQL
    -   レプリカを6個作成し、3つのAvailability Zone(A)に分散配置。
    -   AZが一つ落ちても、4/6は活きているので、多数決できる(quorumを取れる)設計
-   ログ書き込み
    -   MySQLのデータベースの更新操作をログの書き込みだけに特化して簡単にしている
    -   分散された各ノードでログをリプレーして、データベースの状態を同じに保つ
    -   データのコピーには、Gossipプロトコルを採用。送信先ノードをランダムに選び、データをコピーし、だんだん同じ状態に持っていく
    -   トランザクション処理も同期を取らない coordination avoidanceというテクニックを使っていて、2相コミット(2PC)を使わなくても実行できるように設計

Amazon Aurora は非常に洗煉されたデザインをとっているところが面白い。SIGMOD 2018の論文は時間があれば、ぜひ読んでほしい。

[Amazon Aurora: On avoiding distributed consensus for I/Os, commits, and membership changes - Amazon Science](https://www.amazon.science/publications/amazon-aurora-on-avoiding-distributed-consensus-for-i-os-commits-and-membership-changes)

ブログでのSIGMOD 2018論文の解説4部作

1.  [Amazon Aurora under the hood: quorums and correlated failure | AWS Database Blog](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-quorum-and-correlated-failure/)
2.  [Amazon Aurora Under the Hood: Quorum Reads and Mutating State | AWS Database Blog](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-quorum-reads-and-mutating-state/)
3.  [Amazon Aurora Under the Hood: Reducing Costs Using Quorum Sets | AWS Database Blog](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-reducing-costs-using-quorum-sets/)
4.  [Amazon Aurora Under the Hood: Quorum Membership | AWS Database Blog](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-quorum-membership/)

### データの複雑さ

これまではデータベースシステムだけを見てきたが、データから生まれる複雑さが最近の課題としてある。

-   従来のテーブルの意味:
    -   データ ＝ RDMBSにある最新のデータ(snapshot)
-   現代のテーブルの意味:
    -   時間とともに変化するデータ
    -   そこから派生するデータ(derived data)

列指向クエリエンジンの発展により、大量の派生データが出てくるようになってきた。 バッチ処理とストリーム処理の区別がつかなくなくなるほど、いろんなデータが作られるようになっている。

『Streaming Systems』を読むと、色んなパターンが紹介されている

[Streaming Systems : by Tyler Akidau, Slava Chernyak, Reuven Lax](https://www.oreilly.com/library/view/streaming-systems/9781491983867/)

### 導出データ(derived data)

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-derived-data-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-derived-data.jpg)

Treasure Data社の事例

1つのデータから数千の導出データが生成される

クエリで生成されるデータの依存関係、データの履歴の管理が必要。

そのために新しい技術が出てきた

-   dbt:クエリの依存関係を記述するSQLコンパイラ
-   新しいテーブル形式 : Delta Lake, Apache Iceberg, Apache Hudi など
    -   テーブルの更新履歴やスナップショットを管理する需要から開発された

### バッチ処理

#### Unix パイプ処理との類似

データ処理では色々なオペレーターを組み合わせないといけない。 Unixコマンドのデータ処理に非常に近い。

$ cat access.log | grep “xxx.yyy” | awk ‘{ print $2; }’ | sort | uniq

#### UNIX哲学

端的な機能を持つコマンドをたくさん用意し、個々のコマンドをパイプでつないでデータ処理する。 シェルの裏側では、コマンドごとにプロセスが起動し、プロセス間でストリーム処理をしている。 UNIXでコマンドを叩くと、実は分散データ処理が実行されている。

ひと昔前の東大情報科学科のOS演習では、シェルをCで実装する無茶振り課題があり、 プロセス起動、mutex,signal, I/O などの実装が必要で、たくさんの人が挫折した。

### 分散バッチ処理

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-distributed-batch-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-distributed-batch.jpg)

UNIXのパイプのようなものを複数ノードで実行するにはどうすればよいか?

#### Spark(2009)が原典

Spark が原典。 このSpark(2009)も、[Microsoftが開発したDryadLINQ(2008)](https://www.microsoft.com/research/project/dryadlinq/)に影響を受けている。

DryadLINQは`grep` を単純に実行するのではなくて、`grep`を1000台のノードで実行するとか、`sed` を500台で実行するとか、同じコマンドを分散並列実行できる。 これをベースにSparkが作られたという歴史的な順番がある。

#### MapReduce(Google 2004, Hadoop 2006)

さらに昔のよりプリミティブな形として、Google が2004年に発表した MapReduce がある。

[MapReduce: Simplified Data Processing on Large Clusters – Google Research](https://research.google/pubs/pub62/)

MapReduceではMapperが (key, value)のペアを出力し、Reducer側で同じKeyのデータを集めて集計する。 ユーザーはMapperとReducerを書くだけで、フレームワーク側が自動で分散処理してくれる。

-   SQL(Hive)
-   MapSide Join
-   Broadcast/Hash Join

など色々なテクニックが使われている。

### ストリーム処理

#### ストリーム処理とは?

-   SQLのようなものをデータの入力側に送り、常にクエリーを実行し続ける
    -   AWS で言うところの Kinesis Data Analytics
-   データの変更を捉えて、細かい単位のバッチで処理したり(マイクロバッチ)、差分データ(CDC;Change Data Capture)を使って、変更部分に対して処理
    -   AWS で言うところの Kinesis Data Streams

#### Dataflow Model

この辺の概念を上手にまとめたのが、Google が VLDB 2015で発表したエポックメイキングな論文(Dataflow Model)

[The Dataflow Model: A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale, Unbounded, Out-of-Order Data Processing – Google Research](https://research.google/pubs/pub43864/)

-   時系列データ・ストリーム処理の分類・パターンを詳しく定義
-   遅れてやってくるデータ(late arrival)の処理を埋め合わせないといけないことを提示
    -   イベントの発生した時間(event time)とイベントを処理する時間(processing time)をwatermarkで管理して処理。
-   Apache Beamなどに実装されている

### データモデル、クエリ言語の変遷

#### データモデルの歴史

データモデルは歴史から学ぶところが多い。

データモデルは多様で、1970年頃から議論が続いており、詳しい歴史は次のRedbookに書かれている。

-   [Readings in Database Systems (The MIT Press) : Hellerstein, Joseph M., Stonebraker, Michael: Foreign Language Books](https://www.amazon.co.jp/dp/0262693143/)
-   [Readings in Database Systems, 5th Edition](http://www.redbook.io/)

#### 勝者はリレーショナルモデル

昔はネットワークモデル(CODASYL)や階層モデルなど色々なデータモデルが有ったが、**勝者はリレーショナルモデル(RDBMS)**。

ちゃんとトランザクションをサポートするRDBMSにJSON・XMLなど様々なデータ形式が取り込まれていき、RDBMSの一人勝ちになっていった。 結局、トランザクションなどの品質が重要で、トランザクションができないデータベースはユーザーには使えない。

#### インピーダンスミスマッチからのNoSQL

アプリケーションが使いたい表現(グラフ、オブジェクトなど)とデータベースに格納されている形態が異なる。

RDBを使わないといけない不満から、NoSQLというバズワードが出てきたが、筋が悪かった。 NoSQLは「SQLにNo」という意味で始まったけど、次第に「Not Only SQL」というSQL以外もやりたいよねという解釈に変わった。

この歴史的教訓から学んだ良い事例はGraphQL。 GraphQLはRDBMS/SQLとむやみに喧嘩しない。SQLを利用しても、アプリケーションがほしい形でデータを取得する機能を実装しており、非常に好感がもてる。

### SQLの使われ方の変遷

SQLの使われ方も段々変わってきた

-   従来のSQL
    -   RDBMS専用のインターフェース
-   Google F1
    -   VLDB 2018 F1
    -   Google社内の多様なストレージへSQLでアクセス
    -   ZetaSQL:共通SQL実装で、BigQueryの実装にも使われている
-   Meta(Facebook)も同様
    -   共通SQLエンジン
    -   その裏側でSparkやPrestoのデータ処理を統合するVelox処理エンジンをC++で書いている
    -   [CIDR 2023](https://www.cidrdb.org/cidr2023/slides/sponsor-talk-meta-slides.pdf)
    -   [Introducing Velox: An open source unified execution engine](https://engineering.fb.com/2022/08/31/open-source/velox/)
-   Trino Distributed SQL Engine
    -   面白いデザイン
    -   ストレージ実装はなに持たない
    -   さまざまなデータソースへのコネクタを提供して分散SQL(処理部分)だけをOSS化

### 分散データシステムの世界

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-map-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-map.jpg)

分散データシステムの世界を紹介。

広大な世界の地図はオライリーのサイトでダウンロードでき、個々の島は各章に対応。

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-map-ch1-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-map-ch1.jpg)

1章は次がテーマ

-   信頼性
-   スケーラビリティ
-   メンテナンス性

データ指向の意味がわかってくると、これらの重要さもわかってくる。

### Twitterのタイムライン実装

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-twitter-fanout-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-twitter-fanout.jpg)

初期のTwitterでは、ツイートをグローバルなデータベースに格納し、ユーザーがタイムラインを取得するたびに、フォロワーのツイートをDBから検索し、読み込み負荷が高かった。

ツイート時にフォロワーのタイムラインキャッシュに書き込んだところ、書き込みの負荷は増えたが、読み込み負荷が2桁減った。

極端な例として、Twitter CEOのイーロン・マスクには1.28億人のフォロアーがいる。 イーロン・マスクのインプレッションがフォロワー数に対して少ない。イーロン・マスクによると、Fan Outサービスがクラッシュしていて、95％のツイートが配信されていなかった。

> Long day at Twitter HQ with eng team
> 
> Two significant problems mostly addressed:
> 
> 1\. Fanout service for Following feed was getting overloaded when I tweeted, resulting in up to 95% of my tweets not getting delivered at all. Following is now pulling from search (aka Earlybird).…
> 
> — Elon Musk (@elonmusk) [February 12, 2023](https://twitter.com/elonmusk/status/1624660886572126209?ref_src=twsrc%5Etfw)

データ指向アプリケーション的に考察できる箇所が多い、面白い事例

-   信頼性
    -   一部サービスがクラッシュしていても、サービスは落ちていない?
-   メンテナンス性
    -   数千人規模のレイオフをしても、引き継ぎがちゃんとできている?
-   スケーラビリティ
    -   数億回の読み書きする実装はどうなっているの？
    -   性能調整はどうなっている?

### SLO:システムのパフォーマンスをどう測るか?

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-slo-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-slo.jpg)

性能を測る時に大事なのがSLOという概念

システムの平均値を見るだけでは、重要な部分が見えない。 p95、p90の値のレイテンシーを見るといった工夫が必要。

AmazonのSLOでは p99.9のレスポンスタイムを一定値以下に抑えるようしている。

100ms遅くなると、売上が1％下がると言われており、売上が莫大なAmazonの場合、1％でも大きな額となる。 p99.99 まで改善しようとすると、コストに見合わないと判断し、p99.9としている。

### まとめ

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-textbook-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-textbook.jpg)

**データ指向アプリケーションという本は、分散データシステムに入門するための決定版の教科書**。

いろいろな教科書を読んできたなかで、この分野の教科書は2007-2017の10年間、全く存在していなかった。 本書は660ページと分厚いけれども、おのおのの分野の教科書はもっと分厚い。

たとえば、データベースの重要論文を集めた本("Readings in Database Systems;Redbook")は878ページもある。

この辺の本を読む前に、まず「データ指向アプリケーションデザイン」を読み、ざっと概要を把握しておく良い。

### 『データ指向アプリケーションデザイン』が出版されるまで

著者のMartin Kleppmann さんは本書の執筆に多大な努力を払い、執筆に2014年から4年もかけた。 第2版も検討しているが、全く目処が立っていないらしい。 第2版がでるのはだいぶ先ではないか。

この本をガイドにして、より深い世界に踏み込むためのテキストとして使ってほしい。

この本は各章ごとに50-100本の論文が引用されている。

[GitHub - ept/ddia-references: Literature references for “Designing Data-Intensive Applications”](https://github.com/ept/ddia-references)

論文を一個一個読むのは大変なので、まず基礎知識を抑えてから、第2版が出版される10年後に備え、論文から情報収集できるようにしておく。

今はクラウドの時代なので、データ基盤関連のインダストリアルデータが沢山出てきている。

ACM SIGMOD・VLDB・usenix・ICDEなど、データの話が常にある。 これらの論文を読めるようになると面白い。

[![](Clippings/assets/分散データシステム入門の決定版『データ指向アプリケーションデザイン』をたった30分で学んでみた%20DataEngineeringStudy%20%20DevelopersIO-2024-09-13%2013-32-14/data-intensive-further-960x540.jpg)](https://dev.classmethod.jp/wp-content/uploads/2023/02/data-intensive-further.jpg)

以上。

## 質疑応答

キーワードだけ抜粋します。

### 製品の特性を知ること

斉藤:

(BigQueryについて)

アプリケーションがどういう風にデータがアクセスされるかイメージしないといけない。

アクセスした量で課金される

中身を知っていたほうが安心して使える。 SQLしか知らないと、そこまで頭がまわらない

内部の知識を知っていたほうが、より効率的に使える

### dbtやApache Hudiに関連して

斉藤:

(dbtやApache Hudiに関連して)

いろんなシステムがでてきているが、Lineageを追跡するのは結構難しい。 今後はLineageやdbtのようにSQLの依存関係を考慮したシステムが標準になってくると思う。

### 用語の統一

斉藤:

『データ指向アプリケーションデザイン』や『Streaming Systems』のおかげで、用語が統一されてきた 5年前とは違う。

### ハードウェアの進化

斉藤:

(ハードウェアの進化)

ここ10年でハードの進化は大きい

CPUはそんなに速くなっていないが、ネットワークはすごく速くなった。

分散クエリーエンジンの動くEC2のインスタンスタイプを次世代のArmベースのもの(Graviton)に切り替えたら、2-3割性能がはやくなった。

[Performance Metrics: AWS Graviton2 with Presto, M6g - Treasure Data Blog](https://blog.treasuredata.com/blog/2020/03/27/high-performance-sql-aws-graviton2-benchmarks-with-presto-and-arm-treasure-data-cdp/)

### S3依存のデータベース

斉藤:

S3のスループットもどんどん向上し、超並列アクセスしても耐えられるようになり、 **色んな会社がS3の信頼性に依存してシステムを設計**するようになってきている。

DeltaLakeはS3にテーブルカタログを置き、 パーティションのポインターを管理している。

S3の機能だけに頼るようなデザインも起こってきている。

### Amazon Auroraのログが面白い

(次の5年、何が起こるか)

RDBMSではトランザクションは地道に実装し、 分散トランザクションだと、2相Cなどを実装しないとだめだったのに、 Amazon Auroraではログを吐き出すだけで済む。

以前はロックをとらないといけなかったけど、 データを書き出してストレージ・ノードに押し出し、 ストレージノードのあとは勝手にデータを分散して トランザクションを実行してくれるシステムに置き換えることで、 分散ロックとかを気にしなくて済むようになった。

Coordination avoidanceという同期を取らないアプローチがどんどん広がっていきそうな感じがする。

### Query Baseの登場

**「データ」**を管理する **データ**ベースではなく、**データ**にアクセスする**「クエリー」**を管理する **クエリー** ベースが今後でてくるのではないか?

今のdbtはテキストでお遊びをしているようなレベル。

## トークセッション

トークセッションでは、監訳者の斉藤さんと斉藤さんの元同僚でもある田籠さんが参加し

-   本書籍を更に深く学ぶ方法
-   本書籍からの技術進歩に関するアップデートについて
-   本書籍の技術の応用について
-   本書籍を特に読むべき人・タイミングなど

などについて議論します。

大前提として、監訳者の斉藤さんはアカデミア出身で、田籠さんは実務家よりです。 お二人の出自の違いを認識した上で、発言を参考にしてください。

司会進行の小林さんは両者の意見を整理しながら、ニュートラルな立場から発言されています。

### パネリスト

小林 寛和 氏

田籠 聡氏 　[Twitter：@tagomoris](https://twitter.com/tagomoris) / Github：@tagomoris

慶應義塾大学卒業後、株式会社リブセンスにてデータエンジニアとして同社分析基盤立ち上げをリードする。primeNumberへは2017年に参画し、2018年10月にデータ分析基盤の総合支援サービス「trocco」を立ち上げる。以降、同プロダクトの全般を統括し、その運営を担う。

フリーランス・技術顧問

東京大学工学部計数工学科卒。LINEやTreasure Dataなどで働いたのち独立。Webjアプリケーション、ITインフラおよびデータ分析基盤の設計・実装などを専門とする。ISUCONの発案と開始、Fluentd、msgpack-ruby、NorikraなどOSSのメンテナなども。近著に「Fluentd実践入門」(技術評論社)。

### 本書籍を更に深く学ぶ方法

斉藤:

エンジニア的な観点から言うと、データベースを作ってみると、いろんなことが腑に落ちてくると思います。 データベースを真面目に作る人はいないけど。

最近は、簡単に作れるデータベースがたくさん出てきている。

例えば、[DuckDB](https://duckdb.org/)はあえて分散しないデータベースを実装している。

そうすると、分散は無視して、本当にデータのアクセス部分とか、クエリー部分だけ気にすればいい。 しかも、トランザクションもあまりない。

そういうものを作ってみようとすると、B-Tree、カラムナ・ストレージなどの実装で、本書のほとんどの知識が必要になってくる。

コード書くのが好きな人とか、データベースが好きな人は、こういう作る学び方はありと思う。

田籠:

データベースを作ってみようというのは、なかなかハードコアな路線。

本書には、技術的な一般論、典型的な手法を積み上げている。 書いてあることをすべてやれば、良いソフトウェアになるわけではなく、その中でも、取捨選択や分離レベルなどの判断が必要。

製品ごとにどういう選択を用意していて、デフォルトがどういう設定になっているか。 そういった細部に実際の使い勝手や性能特性など大事なものが宿っていることが多い気がする。

RDSMS一つとっても、MySQL中心の人は、別のデータベース(PostgreSQL)やBig Query/Athenaのような分析系エンジンを使ってみると、何が違うのか、何を大事にしているのか、性能的な特性がどう違うのか、といったことがよくわかってくる。

違いがわかった上で、どういう技術から違いがでてきているのか知ると、理解が一段と深まると思う。

斉藤:

2つのアプローチ

-   実装してみる
-   既存の製品を使って学ぶ

キーワードはこの本にたくさんある。

分離レベル、B-Tree、データ構造、レプリケーション、障害耐性

その辺の基礎知識をキーワードだけでも、この本を眺めて拾っておいて、使ってみて、気になった時に、この本に立ち戻るといいと思う。600ページの内容を頭にいれるのは無理。

研究者で論文を読むのが仕事のようなところがあったので、この本が出る前にいろんな技術をすでに知っていたので、本書は論文にかかれていることが上手にまとめてあるなと思った。

ちょっと特殊なケースなので、一般の人には当てはまらないかも。

田籠:

この本が出る前から、仕事していたので、守備範囲ではあった。

論文を読んだものもあるし、いろんなOSSやサービス(Big Query/Aurora)が出てくる時に、内部実装を何らかの形で紹介する。そういうものを読むと、キーワードがあちこち書かれているので、どう実装されているのか、何となく分かる。

キーワードをどれだけ深堀りするかは人によるが、いろんな製品紹介記事を機会があるたびに読むと、自分の中で技術的な蓄積ができる。

斉藤:

僕の学び方としては、Treasure Dataに入る前は24時間動き続けるサービスを作ることなんて無かったので、 リトライとか冪等性はTreasure Dataで経験で学ぶしか無かった。本書にはそのあたりが書いてある。

サービスを提供する側に身を置くと、これらのありがたみ、有効性がよくわかる。

田籠:

作る、手を動かす経験は、どのレベルになっても必要。

小林:

Twitterで質問

> 「データベースを作る」ぐらいの事がそんなハードコアな物扱いされてしまうのが悲しいよね。コード行数や難度としては言語自作や自作OSと大差ない労力でARIES+Selingerぐらいの奴はできるのに

> 「データベースを作る」ぐらいの事がそんなハードコアな物扱いされてしまうのが悲しいよね。コード行数や難度としては言語自作や自作OSと大差ない労力でARIES+Selingerぐらいの奴はできるのに [#DataEngineeringStudy](https://twitter.com/hashtag/DataEngineeringStudy?src=hash&ref_src=twsrc%5Etfw)
> 
> — くまぎ (@kumagi) [February 15, 2023](https://twitter.com/kumagi/status/1625743306788569088?ref_src=twsrc%5Etfw)

斉藤:

データベースを作るのは、段々簡単にはなってきている。 SQLの実装だけなら、山のようにある。

トランザクションの実装もログをシップするだけ

[ARIES](https://qiita.com/kumagi/items/14b6593a2e8ae0c56546) を使うと、分散DBもかんたんに実装できるのではないか?

いまはgRPCのおかげで、RPCもかんたん

分散システムの通信だけなら実装できる。 SQLコンパイラも実装は山のようにある。

データベースの実装は簡単になってきている。 けれども、まずは簡単になっていることを知らないといけない。

田籠:

どういうものが必要なのか頭の中にある人は全体像があるので、それほど高い山に見えない。

全体像がわからないと、みえている範囲の外にどのくらいあるのか分からないし、 全体像の輪郭がわかっても、各々のボリュームが感覚的にわからないと、 恐怖というか、高いハードルに感じてしまう。

CSVをクエリできるSQLエンジンのように、簡単なところから一つ一つ進んでいくのがお勧めのアプローチ。

小林:

本書を読み、データベースの全体像を理解した後でデータベースを作るといいよという斉藤さんの意見に対して、 全体像を理解してもまだまだハードルが高いだろうから、SQLをパースするなど、一部を作るところから始めるといいというのが、田籠さんの意見。

データエンジニアリングの話に特化すると, クラウドDWHは各技術をブラックボックス化してくれていて、 いい意味でも、悪い意味でも、簡単に扱えてしまう。

Q:パネリストは、技術を深める方向にフォーカスしたのか、技術を応用する方向にフォーカスしたのか、どちらだったのでしょうか?

斉藤:

データベースを作ってみたいからキャリアが始まっているので、実用性はあまり考えていなかった。

B-Tree実装を参考にするためにPostgreSQLのソースコードを読むのは大変。 KVSをB-Treeで実装し、ロックもある[Berkeley DB](https://www.oracle.com/database/technologies/related/berkeleydb.html)のコードなら読めて、ドキュメントもしっかりしたので、ここから学ぶことが多かった。

[The Architecture of Open Source Applications: Berkeley DB](https://www.aosabook.org/en/bdb.html)

教科書にはロックを取るとか、シリアライザブルの実装方法は書いてあるけど、データのページレイアウト、ディスクにどう読み書きするかといった細かいところはOSSのコードを読まないとわからなかった。

作ってみたいから始まり、ソースコードをどんどん読むと面白くなっていった。

田籠:

全く逆。

就職して業務システムを作る時に必要に迫られてRDBMSを使い始め、色々なデータベースを使い、仕事の都合で 何百GBあるデータを分析するためにHadoopを使い始めた。

当時はBigQueryのようなマネージドサービスがなかったので、 あるものは組み合わせて使うし、足りないものは自分で作るしかなかくて、 今よりワイルドだったので、作る世界に一歩ずつ足を踏み入れていった。

そういう意味では、出発点が斉藤さんとは全然違う。

本書よりさらに学びたい時、大量データ分析の基盤を自分で作れるかというと、手元にデータが有るか次第。

手元に処理する必要のある大量のデータがある・ないで感覚が全く違う。 大規模データを自前で生成するのは難しい。

斉藤:

データベースの研究者をしばらくやった後、ゲノムサイエンスにうつった。

ゲノムサイエンスは人のDNAを読んで、一人から数TBのデータが出てきて、これを何千人分処理する大規模データ処理の世界。

データを処理しないと何もサイエンスが進まないという世界に放り込まれた。

そこで、DryadLINQやUNIXコマンドで超並列処理するアプローチで大量にデータを処理して統計処理をし、特に、ゲノム配列の検索用のインデックス([FM-index](https://en.wikipedia.org/wiki/FM-index);suffix arrayがベース)を作ったり、現場に飛び込んで、プログラミングの腕も上がったし、データ処理の知識も上がった。

データベースの研究をしているだけだと、実際にデータを使う現場の気持ちがわからなかったけれど、 生物学でータ処理をやり、このあたりが絡み合っていると実感できた。

**データが有る場所に飛び込むのはすごく大事。**

小林:

田籠さんも斉藤さんも全然違うところから山を登った。

### 本書籍からの技術進歩に関するアップデートについて

斉藤:

Parquet、Amazon Aurora、Iceberg、時系列データの変更履歴など書籍でも扱われていたものが、実装レベルで出てきた。

「12章:データシステムの将来」を見ると、データの変更履歴をトラッキングすることの重要性が書かれている。 本書を読んで、実装する流れになったのだと思う。

田籠:

この本の出版時は各パブリッククラウドのデータベース・分析系サービスがほとんど出ていなかったが、この7-8年でぜんぜん違う世界になってきている。

OLTP:

-   Amazon Aurora
-   Google Spanner

OLAP:

-   Athena
-   BigQuery

いろんなサービスが出てきたが、OSSではないので技術詳細がわからない。

斉藤:

クラウドベンダーの中の人しかしらない状況になってきている。 実装の経験が表に出てきていない。

田籠:

理論をもとに実際に実装したとして、実用で求められる一工夫が表に出てきていない。

斉藤:

本書には、Riak(Basho)やRethinkDBなど、会社自体がなくなったデータベースが紹介されている。

OSSとして公開するだけだと、ビジネスとしては続けられない。

**実際にデータを持って、サービスを運用している会社でないと、製品を改善していけない。** 技術の先に、トラフィックから得られる経験をもとに、サービスを改善するフィードバックループまでたどり着かないと、こういったデータ基盤系システムの開発は続けられないんだろうなというのが、この5年間の経験で見えてきている。

RiakのBashoは作った製品を売るだけの立場だった。 実際にデータを集めているAmazonやAzureに比べると、**経験値のたまり方が遅かった**と思われる。

田籠:

当時からあっても本書であまり取り上げられていないものとして、ドキュメントDBがある。 MongoDBやGoogle Cloud Firestoreはサービス(ビジネス)としてはすごくうまくいっている。

利用シェアとしては、ドキュメントDBはだいぶ大きくなっているので、調べる価値があるかも。

本書では、「Dynamoスタイル」という用語が何度が出てくるが、誤解している人が多い。 「DynamoスタイルDB」と「Amazon DynamoDB」は全然異なるデータベース。

「5.4 リーダーレスレプリケーション」から

> レプリケーションを行う初期のデータシステムの中にはリーダーレスだったものもありました。 その発想はリレーショナルデータベースが支配的だった期間にほとんど忘れられていましたが、 Amazon が自社の Dynamo システムで利用した後に再び流行のデータベースアーキテクチャとなりました。 Riak、Cassandra、Voldemort は Dynamo から着想を得たリーダーレスレプリケーションモデルを持つオープンソースのデータストアなので、この種のデータベースは Dynamo スタイルとも呼ばれます。

斉藤:

本が出る少し前に、[CAP定理](https://ja.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)(**C**onsistency; **A**vailability;**P**artition-tolerance)がすごく流行った。

分散システムを利用するにはCAPを理解しないとだめだというような風潮があった。

この本で、CAP定理は意味がないから忘れるよう、ちゃんと書かれている。

CAP定理が流行っていたときは関連論文がたくさん出版されたが、それらは読まなくて良いと、知識をちゃんと整理してくれている。 この本があることで、大昔の論文を漁らなくても済むようになった。

監訳者まえがきから

> 過去に CAP 定理という言葉が流行り、あたかもこれが分散システム設計の前提という風潮になったこともありましたが、この本では CAP 定理を(その対象となるシステムの狭さから)実用上有益ではないと言い切り明確に終止符を打っています。

今後、Dynamoとかのアップデートが必要なので、誰かが整理しないといけない。

小林:

Q:この本の出版後に出てきた技術で、お二人が着目しているものは?

斉藤:

Snowflakeはクラウド(S3)の技術をふんだんに使ったデータベース。 トレジャーデータもほぼ似たようなデータベースがある。

データレイクという話になってくると、技術的には過去に逆行しており、トランザクションの更新はあまり頻繁でなく、アップデートが1分間に数回しかないようなデータ基盤向け。 1秒間に100万回トランザクションが必要だと、このようなデータレイクは動かない。

最先端を突き進んでいるわけではなく、現代的なS3を使って、古い技術を実装しているようなところがある。

田籠:

時系列データベースは本書で扱われていないが、最近はよく使われている。

AWSにもマネージドのAmazon Timestream(2018年Q4発表、2020年Q4 GA)があるし、モニタリング関連のDatadogを始めとして、単独のサービスとして出てきていて、時系列をどう扱うかというのがポイントになってきている。

個人的にも、時系列データを扱っている。

これからデータが減ることはないし、時間の経過に従ってデータが蓄積するものを扱うなら、時系列DBが必要。 [Treasure DataのPlasmaDB](https://qiita.com/xerial/items/959d2b928353b595f31e)もtime-partition orientedなデータベースだった。

小林:

要素要素の技術を見ると、エッジがきいているが、使われているユースケース・ニーズがないと普及しない。

技術的な振り戻しが起きている。

読み込み特化だと、トランザクションは重要ではない。

時系列分析のニーズが伸びていて、AWSも出している。

斉藤:

最近出てくるデータベース関連のサービスは、エンジニアリングやメンテナンス性も意識し、 あえて簡単なデザインにして、長期的にメンテできるような方向性のデータベースも出てきている。

[DatabricksのPhoton](https://www.databricks.com/product/photon)はそのような例。

GoogleのF1もコア部分はC++で書かれているが、SQLのオプティマイザーはPythonで書けるようになっており、いろいろなエンジニアが扱える。

### 本書籍の応用について

小林:

データエンジニアリング、データ分析基盤に特化した事例として、お二人の実業務における、本書籍の応用・事例失敗のようなものは?

斉藤:

データ基盤を使うという観点からすると、データのパイプラインをいかに動かし続けるのかという観点が大事。

データを一回だけ書く(exact once)。そのために、データをクリンナップしてから書いて、冪等にする。

データパイプラインにおいて、システムがクラッシュしたり、間違ったSQLコードをデプロイし、解析結果が壊れた時に、 ロールバックして計算し直すといったことを考えないといけない時代になってきている。

データエンジニアにとって

-   トランザクション
-   冪等

を考える習慣が一般的になってきている。

田籠:

自社向けのデータ分析基盤を作ろうと思った時に、 データパイプラインをどうやって作るかを考えると、元のデータは必ずしもデータ分析用にどこかから湧いてくるわけではない。

本番のデータベースからどのようにデータを分析用に引っ張ってくるか、アプリケーション・サーバーのデータをどうやって集めてくるか、といったことを考える時に、一貫性のあるデータを集めるのが難しい。

複数箇所からデータを持ってきた時に時系列がずれていると、突き合わせてもまともなデータとならない。

タイムスタンプを決めてスナップショットをとったり、整合性のある形でCDCをとってきましょうみたいな話になる。

データをつくるという要素技術だったスナップショットだったりCDCが、データベースの中だけの話ではなく、データ分析基盤を作る時の要素技術として、必要になるケースがよくある。

そういう意味では、本書の内容を前のめりになって使わなくても、それぞれの要素技術が必要になっている。 一度立ち止まり、「これ、『データ指向アプリケーションデザイン』で見たやつだ！」と 振り返り、頭の中でより明確に形作り、現実の仕事に応用すればよい。

斉藤:

データを毎日処理し続けないといけないので、全部のデータをBigQueryなど一つのシステムで処理するのは高くつく。

インクリメンタルに必要なデータだけを処理できるように考えないといけない。

CDC・トランザクションなどは必須の技術になってきている。

斉藤:

データエンジニアも普通のエンジニアがやっているようなCI/CDが必要になってきている。

データベースのクエリに対してテストを書くのはまだ一般的ではないので、知見がたまっていない。

SQLを追加・書き換えるたびにテストを流し、パイプラインがうまくいくか検証し、うまく動いたら本番パイプラインにデプロイし、結果を確認する。うまく行かなければ、どこまでロールバックするか、どこから再計算すればよいか、できるようになると面白い。

コンパイルは、いつも差分コンパイル。 データパイプラインでも、同じようなことをやっていかないといけないが、まだ技術がそこまで揃っていないので、今後変わってくると思う。

### 本書籍を特に読むべき人・タイミングなど

斉藤:

エンジニア・データエンジニアだけでなく、データシステムのマネージャーも読んでみるといい。

キーワードを効率よく抑えられるし、何百本もの論文を読み、OSSコードで学んだことが、この本一冊にまとまっている。

データベースやデータ基盤に興味を持ったら、まずはこの本をパラパラめくり、キーワードを抑えておくと良い。

興味を持ったら、論文を読む基礎体力を鍛えると良い。

最近は、データ基盤系のインダストリー寄りの論文が増えており、BigQueryとかAWSのシステムを使っている人なら、馴染みのあるキーワードが増えている。

田籠:

この分野が気になっているなら、今すぐ読み始めるといい。

一方で、660ページと分厚い。

この分野の技術的なバックグラウンドがないと、ハードな内容の話が続くので、読んでいると疲れるケースが多いと思う。

個人的なお勧めとしては、話としては全部トピックがつながっていて、省略できる話があまりないので、先頭から順番に読んでいくといい。 一方で、ハードな内容を全部飲み込んでから次に行くスタイルだと、挫折する人も多いハズ。

技術的な細部を全部飲み込まないと読み進められないわけではないので、ちょっと難しいと思ったら、その節はななめ読みしても、全体像を頭に入れるという意味では十分。 適度に無理しない範囲で読めるところを読んでいけばいい。

実際に手を動かしていると、この本に戻ってくるタイミングは常にあるので、キーワードだけ頭に入れておいて、読み進め、必要になったら、本書に戻ってくれば良い。

小林:

勉強会で途中で挫折した組なのでよく分かる。

最初からまず読んでみて、難しいところにぶち当たっても、読み進めれば良い。 インデックスを張るような読み方をして、必要になったタイミングで、詳しく読み込む。

斉藤:

Audibleでよむのが一番楽だった。 合計20時間。

この本の監訳に使った時間はその比ではない。 Audibleは止まらないので、わからないところがあっても、20時間あれば、必ず聞き終わる。

### アフタートーク

斉藤：

30分で終わる予定が少し超えた。

わからなかったところを振り返ってほしい。

田籠さんと久しぶりに、データ基盤とかデータの話をできて楽しかった。

田籠：

ここしばらくでは、この分野を振り返る機会がなかったので、斉藤さんと話をできてよかった。 Twitterで反応を貰えるのは、楽しかった。

すごい人が一人いても進まないのがこの世界なので、 みんなでいろいろ違うこと、新しいことを色々やっていけると、面白いと改めて思いました。

## 著者による大学講義のオンライン動画もあり

著者は本書のテーマについて2021年にケンブリッジ大学で学部向けの講義を行っています。

[Department of Computer Science and Technology – Course pages 2021–22: Concurrent and Distributed Systems](https://www.cl.cam.ac.uk/teaching/2122/ConcDisSys/)

合計16回の講義で、前半8回(Concurrent Systems)は David J Greaves が、後半8回(Distributed Systems)は著者のMartin Kleppmannが担当しています。

特に、後半の講義はYouTubeにアップロードされています。

[Distributed Systems lecture series - YouTube](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB)

## 最後に

本書の読書会では怒涛の情報量に圧倒され、キーワードを叩き込むだけでパッツンパッツンでしたが、その読み方もあながち間違っていなかったとホッとしました。わからないところがあっても先に進み、読み通すことを優先しましょう。一人で読むと挫折しやすいタイプの本であることは確かなので、仲間を誘って読書会をするのがお勧めです。

現代的なトピックや時系列DBのように抜け落ちている技術をさらいつつ、業務関連のテーマについても本書を読み返して理解を深めたいと思います。

自作データベースをバケットリストから消し込む目処が全く立っていないので、なんとかしたいところです。

それでは。

## 脚注