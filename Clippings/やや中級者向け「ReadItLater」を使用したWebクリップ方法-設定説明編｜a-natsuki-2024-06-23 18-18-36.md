# やや中級者向け「ReadItLater」を使用したWebクリップ方法-設定説明編｜a-natsuki
  #ReadItLater 
 #ReadableArticle
## articleURL
https://note.com/natsuki_ass/n/nb402db73e3d5

## siteName
note（ノート）

## date
2024-06-23

## articleContent
Image by AI素材.com

Obsidianには「**ReadItLater**」というWebページをクリップできるプラグインがあります。  
これを使用すると簡単な手順でObsidianにWebページをクリップしたノートを作成することができます。

> URLをコピー  
> ↓  
> Obsidianのサイドメニューボタンをクリック  
> ↓  
> 設定した場所に設定した書式で全ページの内容のノートができる。

こちらの記事では「**ReadItLater**」を使う際の設定方法について説明してみました。  
Obsidianのプラグインを入れたことがないあなたにはこちらを(・ω・)つ旦

  

## とりあえずとっとと試してみたいあなたへ全部盛り設定方法

とりあえず使ってみてカスタマイズするあなたへ(・ω・)つ旦

### Readable Articleの設定

-   **Readable article note template titleの設定**
    

「**Readable article note template title**」の横の入力欄に下記をコピペ。

```
%title%-%date%
```

-   **Readable article note templateの設定**
    

「**Readable article note template**」の横の入力欄に下記をコピペ。

```
# %articleTitle%
  #ReadItLater 
 #ReadableArticle

## articleURL
%articleURL%

## siteName
note（ノート）

## date
%date%

## articleContent
%articleContent%
```

-   **オプションのオンオフ**
    

「**Downlord images**」オプションをオン。  
「**Downlord images to note folder**」オプションをオン。

## ReadItLaterの設定について

ReadItLaterは「オプション」で自分の好みに設定できます。

設定を開くと左メニューに「コミュニティプラグイン」という項目が増えていますので、この中からReadItLaterを選択します。

![](https://assets.st-note.com/img/1690060906582-j3VWLf2Jj2.jpg?width=800)

ReadItLaterは様々なWebサイト別の設定ができますが、今回は汎用のWebページをクリップするための設定について説明していきます。

### General

-   **Inbox dir**
    

クリップしたノートが保管されるフォルダを設定します。  
デフォルトだとVolteの直下に「**ReadItLater Inbox**」というフォルダが作成されそこに保管されます。

-   **Assets dir**
    

クリップしたWebノートについている画像ファイルなどが保管されるフォルダを設定します。  
デフォルトだとVolteの直下に作られた「**ReadItLater Inbox**」というフォルダ（「Inbox dir」の設定）の直下に「**assets**」というフォルダが作成されそこに保管されます。

-   **Open new note**
    

Webノートのクリップに成功するとそのノートが開かれます。

-   **Date format string**
    

テンプレートに使用する日付のフォーマットを決定します。  
この項目の日付フォーマットはノート（ファイル）名の変数にのみ影響します。  
日付のフォーマット書式についてはこちらを参照(・ω・)つ旦

**Date format String in contents**

テンプレートに使用する日付のフォーマットを決定します。  
この項目の日付フォーマットはノート（ファイル）内容の変数にのみ影響します。  
日付のフォーマット書式については上項目参照。

-   **Extend share menu**
    

Shereメニューの有無を切り替えるオプションみたいなのですがちょっとわかりませんでした。  
誰か教えて(・ω・)つ旦  
Obsidianを再起動しないと反映されないとの注意書きもあります。

### Readable Article

汎用の（読み取り可能な）Webページのクリップ時に有効になる設定項目です。  
だいぶ設定の下の方にありますが、この上にあるyoutubeなどの特殊なサイトの場合、そちらの設定が優先されます。

-   **Readable article note template title**
    

Webページをクリップしたときの作成されるノート（ファイル）の名前を設定します。  
入力欄に入力した文字がそのままファイル名に反映されます。  
変数も使え、通常そちらを使ってユニークな（ダブらないようなわかりやすい）名前を自動で取得して保存できます。  
こを削除して何も書いていない状態になると名前のないファイル「（**ここに何もない).md**」ができてしまいます。  
このファイルはObsidian上では表示されないので注意が必要です。

![](https://assets.st-note.com/img/1690065449266-1weLPDv6CN.jpg?width=800)

こういうの出来ちゃうとめっちゃ気になる。

また、変数を使って作成するファイル名をつど変更してあげないと同名のファイルになってしまい2回目以降（違うWebページをクリップしても）ノートが作成されません。

-   **Readable article note template**
    

Webページをクリップしたときの作成されるノート（ファイル）の内容を設定します。  
入力欄に入力した文字がそのままノート内容に反映されます。  
変数も使え、そちらを使うと内容以外にもタイトルやURLなども取得できます。  
またここに書いた内容はマークダウン記法に従いそのままノートに反映されるため、フロントマターを自動で入れておいたり、取得した内容に好みの修飾を自動で付けたりできます。

-   **Downlord images**
    

「**Downlord images**」オプションをオンにすると「**assets**」フォルダ（デフォルトだと）に画像が保管されます。  
またその際に「**webp**」のような直接Obsidianで読み込めない書式の画像を「**jpg**」に変換してくれます。

-   **Downlord images to note folder**
    

「**Downlord images to note folder**」オプションで先程のフォルダに作成ノート名と同じフォルダが作成され、そこに画像が保存されます。

### Readable article note templateなどに使用できる変数について

上記の設定の入力欄に使用できる変数についてはこちら(・ω・)つ旦