---
Created: Invalid date
URL: https://dev.classmethod.jp/articles/should_i_choice_s3_storage_class/
---
[![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/amazon-s3.png)](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/05/amazon-s3.png)

---

この記事は公開されてから1年以上経過しています。情報が古い可能性がありますので、ご注意ください。

ご機嫌いかがでしょうか、豊崎です。

S3のストレージクラスは現在6つあります。どういったユースケースでどのS3のストレージクラスを選択したら良いのか、整理したかったので、チャートを作ってみました。

それではまずはS3の種類からです。

## S3の種類と特徴

執筆時点でS3には以下6つのストレージクラスが存在します。（正確にはAmazon S3低冗長化ストレージ(RSS)を含めると7種類ですが、推奨されていないため、除外し6種類と記載しています。）

- S3 Standard
- S3 Standard-IA
- S3 Intelligent-Tiering
- S3 One Zone-IA
- S3 Glacier
- S3 Glacier Deep Archive

## 6つのS3ストレージクラス

### 特徴

それぞれ以下のような特徴があります。

### S3 Standard

- 頻繁にアクセスされるデータ向け

### S3 Standard-IA

- 存続期間が長くあまり頻繁にアクセスされないデータ向け

### S3 Intelligent-Tiering

- アクセスパターンが変化、または不明な存続期間が長いデータ向け

### S3 One Zone-IA

- 存続期間が長くあまり頻繁にアクセスされない、且つ重要度の低いデータ向け

### S3 Glacier

- 取得時間が数分から数時間許容される長期アーカイブデータ向け

### S3 Glacier Deep Archive

- 取得時間が12時間許容される長期アーカイブデータ向け

### 特徴的な差異

S3のデータの耐久性（データロストの可能性）はこれら全てのストレージクラス共通で99.999999999%、いわゆるイレブンナインです。また、S3の可用性（利用可能な時間）は別に設定されていますので、可用性を含む差異を表に示します。※表で示した以外の詳細については[AWSドキュメント](https://docs.aws.amazon.com/ja_jp/s3/index.html)を併せてご確認ください。

|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|ストレージクラス|Standard|Standard-IA|Intelligent-Tiering|One Zone-IA|Glacier|Glacier Deep Archive|
|可用性|99.99%|99.9%|99.9%|99.5%|99.99%|99.99%|
|保存されるAZ|**3AZ以上**|**3AZ以上**|**3AZ以上**|1AZ|**3AZ以上**|**3AZ以上**|
|データ取り出し費用|なし|なし|**あり**|**あり**|**あり**|**あり**|
|モニタリング費用|なし|なし|**あり**|なし|なし|なし|
|オートメーション費用|なし|なし|**あり**|なし|なし|なし|

さて簡単に利用用途と特性が理解できたところでどのストレージクラスを選ぶべきかのチャートを書いていきたいと思います。

## S3診断チャート

## 参考

[Amazon S3 ストレージクラスを使用する](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/userguide/storage-class-intro.html#sc-compare)

## さいごに

意外と分岐なかったですねw。6つあるS3ストレージクラスのどれを選ぶのが良いのか？判断を迷う時にご参考にしていただけますと幸いです。