---
finished reading: true
favorite: false
tags:
  - clippings
---
# Chrome拡張の高速な英語辞書ツールをつくりました(Mouse Dictionary) - Qiita
  #ReadItLater 
 #ReadableArticle

## articleURL
https://qiita.com/wtetsu/items/c43232c6c44918e977c9

## siteName
Qiita

## date
2025-02-01

## articleContent
## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#mouse-dictionary)Mouse Dictionary

Chrome拡張の高速な英語辞書ツールです。

ダウンロード:

-   [Chrome版](https://chrome.google.com/webstore/detail/mouse-dictionary/dnclbikcihnpjohihfcmmldgkjnebgnj)
-   [Firefox版](https://addons.mozilla.org/ja/firefox/addon/mousedictionary/)

とりわけ、ブラウザでよく技術文書を読む人に最高の体験を提供できるように設計していますが、もちろんどなたでもご活用いただけます。

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F57de929d-5f4b-22d1-2cfc-3c2cd78ee509.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=00881f4dd5a04921dd8d6cdb410f9ecd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F57de929d-5f4b-22d1-2cfc-3c2cd78ee509.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=00881f4dd5a04921dd8d6cdb410f9ecd)

PDFも対応  
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F864a7856-f968-0633-dd0d-eff7dd48e2b8.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8e1710e61db1b6c45fb970d53a59008e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F864a7856-f968-0633-dd0d-eff7dd48e2b8.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8e1710e61db1b6c45fb970d53a59008e)

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E7%89%B9%E5%BE%B4)特徴

Mouse Dictionaryが最重視していること:

1.  とにかく高速な体験を提供。1/60秒で辞書引き完了(たぶん理論上最速)
2.  単語や熟語の検出に強い
3.  好きな辞書データを簡単にインポートできる(熟語データの豊富な英辞郎オススメ)

ブラウザの辞書ツールは1と2の特徴が本当に重要だと思っていて、これができると知らない表現を覚えることができる機会が圧倒的に増えます。

その他、以下のような特徴もあります。

-   **PDF文書にも標準対応**
-   YouTube等の英語字幕にも使える
-   テキストボックス内の文章にも使える
-   日英や英英にも対応(後述)
-   日本語解析もそこそこ賢い
-   技術文書にありがちなcamelCase, snake\_caseみたいな表記にも問題なく使える

**「単語や熟語の検出に強い」** については、以下のような感じです。

| 例 | マウス下のテキスト | 引ける語 |
| --- | --- | --- |
| 単語にバラす | camelCase | camel case |
| 単語にバラす | snake\_case | snake case |
| 不規則変化な過去形を含む | dealt with | deal, deal with |
| 単語が繋がっていても熟語として認識 | spinout | spin out |
| 途中に形容詞などが挟まる場合 | make some modifications | make a modification |
| 所有格 | changed our mind | change one's mind |
| 綴りスタイルの違い | programme | program |
| 上記の他にもいろいろ | pros/cons | pros and cons |

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E7%B5%8C%E7%B7%AF)経緯

もともと[MouseoverDictionary](https://addons.mozilla.org/ja/firefox/addon/mouseoverdictionary/)という素晴らしいFirefox用辞書があったのですが、Quantumの登場とXULの廃止とともに使えなくなってしまったため、自分用にChrome拡張をつくった次第です。

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E3%82%BD%E3%83%BC%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%89)ソースコード

実装に関わる技術寄りの用語: React, esbuild, chrome.storage.local, chrome.storage.sync, Cross-extension messaging, Hogan, debounce, resizable/draggable, intl.v8BreakIterator, deinja, クロスブラウザ, など。

※詳細は「Mouse Dictionaryの技術的な話」をご参照ください  
[https://qiita.com/wtetsu/items/2a5568cb0b5a38c003fb](https://qiita.com/wtetsu/items/2a5568cb0b5a38c003fb)

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E4%BD%BF%E3%81%84%E6%96%B9)使い方

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)インストール

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E8%BE%9E%E6%9B%B8%E3%83%87%E3%83%BC%E3%82%BF%E3%81%AE%E4%BD%9C%E6%88%90)辞書データの作成

Mouse Dictionaryのアイコンを右クリック→オプションで設定画面を開く。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2Fb4c82442-d165-a5f1-5646-ff66db04bfcd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b54de9c8aa29f4f0f80f598c41103025)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2Fb4c82442-d165-a5f1-5646-ff66db04bfcd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b54de9c8aa29f4f0f80f598c41103025)

初めて設定画面を開くとき、こういうことをきかれます。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F081557c1-c1d9-e483-917a-ddea5cdd8fb9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=281981d93a30c035334ff3896777fb1f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F081557c1-c1d9-e483-917a-ddea5cdd8fb9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=281981d93a30c035334ff3896777fb1f)

OKを押すと、自動的に辞書データが作成されます。  
([ejdic-hand](https://github.com/kujirahand/EJDict)データが添付されています)

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Faa616da4-9143-7b7c-dfe5-a76e2307086d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3a6979406cec363520a5b8d2baeb34c7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Faa616da4-9143-7b7c-dfe5-a76e2307086d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3a6979406cec363520a5b8d2baeb34c7)

こうなったら成功です。

適当なページで、Chromeの右上に表示されていると思われるこの拡張のアイコンを押すと、

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F43cf3bf6-7173-6a33-1acc-15436ba9967b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2011cfee10f176789e4ffe58a40574e3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F43cf3bf6-7173-6a33-1acc-15436ba9967b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2011cfee10f176789e4ffe58a40574e3)

こんな感じになります。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fb7dabca0-e215-8501-1d6e-2ab0a7adba00.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8dec54c3245bfe5e59cf7d53208445b1)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fb7dabca0-e215-8501-1d6e-2ab0a7adba00.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8dec54c3245bfe5e59cf7d53208445b1)

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E8%8B%B1%E8%BE%9E%E9%83%8E)英辞郎

このままでも46,971語のデータが入り便利にご活用いただけますが、熟語などもたくさん含んだ辞書データをインポートすると、格段に便利になります。

英辞郎が大変おすすめです。

というかむしろこの拡張は英辞郎を使うことを目的に作りました。なんとこんな素晴らしいものが500円以下です。。。

「Shift-JIS」「英辞郎テキストデータ」を選択し、辞書データファイルを選択して「LOAD」を押してください。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F69a74a47-8323-9d07-5272-20097c57d184.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3130c94e384ac9e0ff71dd3377fc6c10)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F69a74a47-8323-9d07-5272-20097c57d184.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3130c94e384ac9e0ff71dd3377fc6c10)

インポートにはさすがにちょっと時間がかかります。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fb398cd02-fbf4-b041-9652-3b0086ecab3a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=30a9e07033bc8fbc793095138851c9b1)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fb398cd02-fbf4-b041-9652-3b0086ecab3a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=30a9e07033bc8fbc793095138851c9b1)

終わりました。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F7888beb5-1d88-cddd-d920-11407dae65ab.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f9bf3d3f364844ad62e522fb6149200d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F7888beb5-1d88-cddd-d920-11407dae65ab.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f9bf3d3f364844ad62e522fb6149200d)

これで熟語もザクザクひけます。

[![md_10.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fa1cc7a8a-6abb-3393-e823-957ed182727e.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=91dcb3daecd9f0e59749a7137052f441)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2Fa1cc7a8a-6abb-3393-e823-957ed182727e.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=91dcb3daecd9f0e59749a7137052f441)

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E8%B5%B7%E5%8B%95%E3%82%B7%E3%83%A7%E3%83%BC%E3%83%88%E3%82%AB%E3%83%83%E3%83%88%E3%82%AD%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E8%BF%BD%E8%A8%98)起動ショートカットキーの設定(追記)

Chromeで↓を開いてください。

chrome://extensions/shortcuts

あとは、こんな感じで設定できます。

[![image.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F45872162-4d2e-aab3-300e-4a5c767d477a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8c623b0b4f9d985c6e3ddb7c24f05aba)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F106401%2F45872162-4d2e-aab3-300e-4a5c767d477a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8c623b0b4f9d985c6e3ddb7c24f05aba)

個人的には、右手でマウスを操作しながら起動したいので、左手だけで押しやすいショートカットキーにしています。

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#pdf%E3%81%A7%E3%81%AE%E5%88%A9%E7%94%A82020%E5%B9%B48%E6%9C%88%E8%BF%BD%E8%A8%98)PDFでの利用(2020年8月追記)

Mouse DictionaryはPDF文書に対しても使うことができます。これは他の辞書ツールにはない機能かと思います。

この機能をどのように実装したかの技術的な話→(追記予定)

Web上のPDF文書に使いたい場合は、通常通りMouse Dictionaryを起動してください。

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F57de29d2-fdb7-a5b5-1446-979bb927829a.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c4c3414af564141ae8c4b3c730ed9175)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F57de29d2-fdb7-a5b5-1446-979bb927829a.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c4c3414af564141ae8c4b3c730ed9175)

ローカルのPDF文書に使いたい場合は、オプション画面からPDFビューアを起動して、ドラッグ&ドロップで開いてください。  
(頻繁に使う場合はPDFビューアをブックマークしておくと便利)

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F36377d3b-53e4-9398-92a3-aac5e7469e0d.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fa1cd90a4605a9b718507874c7d3d1dc)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F36377d3b-53e4-9398-92a3-aac5e7469e0d.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fa1cd90a4605a9b718507874c7d3d1dc)

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E6%97%A5%E8%8B%B1%E3%83%87%E3%83%BC%E3%82%BF%E3%82%84%E8%8B%B1%E8%8B%B1%E3%83%87%E3%83%BC%E3%82%BF)日英データや英英データ

Mouse Dictionaryは日本語テキストもそこそこうまく解析します。

(これはカーソルを端にかざしているのにもかかわらず「ウィキペディア」という語を第一に表示してくれているの図)

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F6bcfcd96-b57a-ea64-0405-3f1f5660136f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bf4d2ee21ffda003a8d23356b85ed13f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F6bcfcd96-b57a-ea64-0405-3f1f5660136f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bf4d2ee21ffda003a8d23356b85ed13f)

簡単にインポートできる日英データを用意しました。UTF-8 + TSVでインポートしてください。  
[Dictionary data for Mouse Dictionary](https://mouse-dictionary.netlify.com/data/)

(使用例)  
[![md_04.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F32ae1e39-cb98-42be-dce7-dcc72d713b96.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c14bf9b115a0b60e4664d54111066eff)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F106401%2F32ae1e39-cb98-42be-dce7-dcc72d713b96.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c14bf9b115a0b60e4664d54111066eff)

その他の簡単にインポートできるデータの情報はこちら。

私は、上記の日英、英辞郎(英日)、略語郎をインポートしています。

他、Mouse Dictionaryで使えれば便利そうなデータがあったら教えていただければ幸いです。

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E3%81%9D%E3%81%AE%E4%BB%96)その他

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#kaggle%E3%81%A7%E5%8B%95%E3%81%8B%E3%81%AA%E3%81%84)Kaggleで動かない

iframeのせいです。Cross-extension messagingという仕組みで解決したので、詳しくはこちらをご覧ください。

[KaggleでもMouse Dictionaryを使えるようにする](https://qiita.com/wtetsu/items/ec2b0fd0c5e379e9bb1a)

この仕組を利用すれば、別の拡張からMouse Dictionaryにテキストを送ることができます。これを応用すれば、たとえばですが、画像内のテキストを取得してMouse Dictionaryにテキストを送る、みたいな別拡張があれば、Mouse Dictionaryはそれ自身の機能拡張なしに、画像テキスト対応することができるようになるわけです(想像上の話です)

そももそKaggle以外のiframeなサイトにも対応したい場合は、こちらをインストールしてください。

-   [Mouse Dictionary iframe support](https://chrome.google.com/webstore/detail/mouse-dictionary-iframe-s/nigglogmamjbcnljijokibobpcfgmdfn)

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#evernote%E3%81%A7%E5%8B%95%E3%81%8B%E3%81%AA%E3%81%84--%E7%B7%A8%E9%9B%86%E3%83%A2%E3%83%BC%E3%83%89%E3%81%AEconfluence%E3%81%A7%E5%8B%95%E3%81%8B%E3%81%AA%E3%81%84)Evernoteで動かない / 編集モードのConfluenceで動かない

同上の理由です。↓をインストールすれば動きます。

-   [Mouse Dictionary iframe support](https://chrome.google.com/webstore/detail/mouse-dictionary-iframe-s/nigglogmamjbcnljijokibobpcfgmdfn)

### [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E8%A1%A8%E7%A4%BA%E3%82%92%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA%E3%81%97%E3%81%9F%E3%81%84)表示をカスタマイズしたい

見出し語をクリックすると画像検索とかSkELLとかに飛ぶようにすると便利です。

HTMLの知識があれば、mustacheスタイルのテンプレートでいろいろいじれるようになっています。内部的にはHogan.jsを利用しています。

詳しくはこちら。  
[HTML templates](https://github.com/wtetsu/mouse-dictionary/wiki/HTML-templates)

## [](https://qiita.com/wtetsu/items/c43232c6c44918e977c9#%E3%83%AA%E3%83%B3%E3%82%AF)リンク

-   [公式サイト(日本語)](https://mouse-dictionary.netlify.app//ja/)
-   [公式サイト(英語)](https://mouse-dictionary.netlify.app//en/)
-   [Getting started](https://github.com/wtetsu/mouse-dictionary/wiki/Getting-started)
-   [HTML templates](https://github.com/wtetsu/mouse-dictionary/wiki/HTML-templates)
-   [Advanced features](https://github.com/wtetsu/mouse-dictionary/wiki/Advanced-features)