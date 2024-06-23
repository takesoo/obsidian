---
finished reading: true
---

# 【Obsidian】Dataviewプラグインでノートの検索・抽出を自動化する | ほしぱそ。
  #ReadItLater 
 #ReadableArticle

## articleURL
https://hoshipaso.com/obsidian_dataview_basic/

## siteName
ほしぱそ。

## date
2024-06-23

## articleContent
![](Clippings/assets/【Obsidian】Dataviewプラグインでノートの検索・抽出を自動化する%20%20ほしぱそ。-2024-06-23%2019-18-49/obsidian_ogp.webp)

Obsidianの記事はいくつかあれど、Dataviewプラグインの基本を押さえている記事は少ない印象。ということで今回は備忘録として、Dataviewプラグインに触れていこうと思います。

目次

## Dataviewプラグインについて

結論から言いますと、「こちらが指定した条件式に応じてテキストファイルを自動で検索・結果を表示する」プラグインです。とくにフロントマターを使ったデータ検索が可能です。具体的に言うと、下記のようなものが挙げられます。

-   フロントマターに指定した日付順に並べる
-   フロントマターAに入ってる文字列Bを抽出する
-   フロントマターCの数字を基準に一覧結果を並べる

＜余談＞  
Dataviewはテキストファイルだけではなく、タスクの抽出・管理もできます。  
ただ、私がタスク管理方面で使ったことがないので今回は省略します。

## Dataview の主な型

````
``` dataview
    table or list
    (table指定時のみ) hoge as "foo", hoge as "foo"
    from "hoge" and #foo
    where hoge = "foo"
    sort hoge asc
```
````

### 1.dataviewの宣言(1行目)

コードブロックを記載し、二行目にデータビューの宣言を行ないます。

### 2.tableもしくはlistの指定（2行目）

次にどの形式で一覧を表示するか設定します。今回はテーブルもしくはリストの表示を選択。

### 3.（tableの場合）列の指定（3行目）

テーブル形式の場合、ファイル名以外にも列を追加できます。hogeの部分にフロントマターを、fooの部分に見出しに入れたい文字を入れます。2つ以上の列を追加したい場合、カンマ（,）で区切って設定できます。

### 4.メモファイルを探す範囲（4行目・フォルダーもしくはタグ）

「from」の行はメモを探す範囲が指定できます。フォルダーもしくはタグが対象です。

#### Inboxフォルダーを指定する

`from "Inbox"`

#### 保管庫全体のメモの中から#Tagを探す

`from #Tag`

#### Inbox/Testフォルダーにあるファイルかつ#Tagを指定する

`from "Inbox/Test" and #Tag`

### 5.詳しい条件文を指定する（5行目）

「where」でフロントマターを使用した細かい条件文を作成できます。

#### フロントマターhogeに入ってる項目がfooのメモを表示

`where hoge = "foo"`

#### フロントマターhogeの数値が10以上の場合、メモを表示

`where 10 < hoge`

#### フロントマターhogeの数値が1以上、10以下の場合、メモを表示

`where 1 < hoge and hoge < 10`

### 5.一覧結果をソートする（6行目）

「sort」の項目で一覧結果の順番を設定します。フロントマターhogeを数値や文字列でソートし、「asc＝昇順・desc＝降順」で表示します。

## メタデータを使う

Dataviewプラグインによって、いくつかのメタデータが自動でインデックスされます。そのため、メタデータに保存されている情報を使って、検索に利用・ソートできます。主に使われるメタデータは下記のようなものがあります。

-   file.name（ファイル名）
-   file.cday（ファイルが作成された日付）
-   file.etags（メモ内にあるタグのリスト）

### 例：今日作成したメモを検索する

`where file.cday = date(today)`

詳しいメタデータリストは公式ドキュメント（英語）を参照してみてください。

[Metadata on Pages – Dataview](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-pages/)

## 試しに作ってみる

### 例1：Inboxフォルダーに入っているメモを抽出・日付順にソートする（table）

````
``` dataview
    table
    from "Inbox"
    sort file.cday asc
```
````

### 例2：InboxフォルダーかつMOCタグが付いているメモを抽出・日付順にソートする（list）

````
``` dataview
    list
    from "Inbox" and #MOC
    sort file.cday asc
```
````

### 例3：Inboxフォルダーかつ今日作成したメモを抽出・日付順にソートする(list)

````
``` dataview
   list
   from "Inbox"
   where file.cday = date(today)
   sort file.cday asc
```
````

## 最後に

Dataviewプラグインに関しては、「習うより慣れろ」な部分が大きいです。上記のサンプルをいじりながら、「Dataviewプラグインってこんな感じなんだ～」と、ふんわり覚えていただけるだけでもうれしいです！

当ブログでは、Dataviewの関連記事も書いています。こちらも参考にしていただけると幸いです。

あわせて読みたい

![](Clippings/assets/【Obsidian】Dataviewプラグインでノートの検索・抽出を自動化する%20%20ほしぱそ。-2024-06-23%2019-18-49/obsidian_ogp.webp)

[【Obsidian】Dataviewのファイル名をエイリアス表記にする](https://hoshipaso.com/obsidian_dataview_alias/) ユニークIDで運用していると、ファイル名が数字の羅列になってしまって、中身の確認が非常に面倒です。そのため、ファイル名をエイリアス表記にしたいと思う人がいるの…

あわせて読みたい

![](Clippings/assets/【Obsidian】Dataviewプラグインでノートの検索・抽出を自動化する%20%20ほしぱそ。-2024-06-23%2019-18-49/obsidian_ogp.webp)

[【Obsidian】Dataviewプラグインで正規表現検索を行う](https://hoshipaso.com/dataview_regex_search/) Dataviewで正規表現検索ができないかと情報を漁ってたら、見事にハマりました。結構苦戦したので（1・2時間くらい）今回怨嗟を込めてブログに記そうと思います。 まあ、…

## 参考リンク

-   [Dataview](https://blacksmithgu.github.io/obsidian-dataview/)