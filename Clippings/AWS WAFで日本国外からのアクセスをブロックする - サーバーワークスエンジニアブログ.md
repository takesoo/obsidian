---
Created: Invalid date
URL: https://blog.serverworks.co.jp/aws-waf-block-from-overseas-countries
---
[![](https://cdn-ak.f.st-hatena.com/images/fotolife/s/swx-watanabe/20200825/20200825193346.png)](https://cdn-ak.f.st-hatena.com/images/fotolife/s/swx-watanabe/20200825/20200825193346.png)

---

ALBへのアクセスを日本国内からに限定したいということがあります。 AWS WAFに以下のルールを設定すれば簡単です。

## Web ACLにルールを追加

Web ACLの設定画面で **Add my own rules and rule groups** を選択します。

## Rule builderで設定を入れる

Rule builderの画面になります。 ソースIPアドレスが、JP以外からのアクセスをブロックするルールです。

### Rule builder

### Rule

|   |   |
|---|---|
|設定項目|設定値|
|Name|任意の名前|
|Type|Regular rule|

### If a request

設定値

---

doesn't match the statement (NOT)

---

### Statement

|   |   |
|---|---|
|設定項目|設定値|
|Inspect|Originates from a country in|
|Country codes|Japan - JP|
|IP address to use to determine the country of origin|Source IP address|

### Then

### Action

設定値

---

Block

---

## 設定の保存

今回は、他には何もルールがない初期状態のWeb ACLなので、そのまま **Save** します。 もし、他にルールがある場合は、**Move up** または **Move down** でWeb ACL内のルール適用の順番も調整します。

## Web ACLの設定完了

こんな画面です。

## 動作テスト

海外からのアクセスを本当にブロックするか確認してみます。 海外の人にアクセスしてもらったり、VPNを使ったりといろいろ方法はありますが、今回はシドニーリージョンのCloud9からcurlでアクセスしてみました。

期待通り、403 Forbidden になりました。

なお、今回はWeb ACLの中にRuleを1つ作成するという方法を選択しました。別の方法としては、Rule Groupというのを作り、それをWeb ACLに適用もできます。複数Web ACLがある場合はRule Groupの方がいいかもしれません。