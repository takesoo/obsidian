---
category: "[[Clippings]]"
author: "[[創造性原理]]"
title: "[Obsidian] TagFolderとタグの使い方に迫る！ (対談企画03)"
source: https://pouhon.net/obsidian-tagfolder/7552/
clipped: 2023-08-20
published: 2022-10-03
topics: 
tags: [clippings ソフトウェア]
---

「セレンディピティ」とは、  
偶然の産物、たまたま出会えた幸運、またはそれを手に入れる力と訳される単語である。

頭を柔軟にしておくことだ。そうすればObsidianのコミュニティプラグイン「**TagFolder**」は、あなたに「セレンディピティ」の何たるかを体感させてくれるだろう。

[

![](https://opengraph.githubassets.com/4ab9d67451d8cf7c3f929e83398bdb481a8f8a92d93f4910999b2a6ab37de013/vrtmrz/obsidian-tagfolder)

GitHub - vrtmrz/obsidian-tagfolder

Contributetovrtmrz/obsidian-tagfolderdevelopmentbycreatinganaccountonGitHub.





](https://github.com/vrtmrz/obsidian-tagfolder "GitHub - vrtmrz/obsidian-tagfolder")

その基本機能は、至ってシンプルである。

-   **タグをフォルダとして扱う**

わけがわからない？ 何が良いのかイメージできない？  
そんなあなたにTagFolderを解説してくれるのは、「[Self-hosted LiveSync](https://github.com/vrtmrz/obsidian-livesync)」でおなじみのプラグイン開発者 **vorotamoroz (ヴォロータ・モローズ) aka きみのぶ**氏！

[

![](https://s.wordpress.com/mshots/v1/https%3A%2F%2Ftwitter.com%2Fvorotamoroz?w=320&h=180)

https://twitter.com/vorotamoroz





](https://twitter.com/vorotamoroz "https://twitter.com/vorotamoroz")

思いもしなかった偶然の出会い、その喜びを共に分かち合おう。

「迫る！」シリーズはこのブログにソフトウェア開発者を迎え、執筆者ぷーおんが開発者へのインタビューを通してプログラムを丸裸にしていく、**開発者 vs ユーザー**の対談企画である。

## TagFolderの基本機能

Pouhon: 今回も[前回記事](https://pouhon.net/selfhosted_livesync/7470/)に引き続き、きみのぶさんにゲストにお越しいただいております。よろしくお願いします。

きみのぶ: よろしくお願いします。

Pouhon: 今回取り上げるのは「TagFolder」ですね……「Self-hosted LiveSync」もそうなんですが、まぁクセがすごいと(笑)  
何か良さげだなというのはわかるんだけど、なかなか馴染めない方も多いんじゃないかということで、これは開発者に直接お話をうかがうのが最も近道ではないかと。そんなわけで、まずTagFolderの導入方法と基本機能について教えていただきたいと思います。

きみのぶ: はい。LiveSyncと違って導入自体はすごく簡単で、プラグインのインストール後、コマンドパレット「Show Tag Folder」で起動します。  
起動すると左サイドバーにTag Folderペインが表示されるので、クリックして閲覧してください。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928130632.jpg)

### タグのディレクトリ化

Pouhon: 実際このペインには何がどんなふうに表示されるんでしょうか？

きみのぶ: 1つ簡単な例を挙げてみましょう。ここに『赤くて甘い』りんご.mdと『甘い』もも.mdがあったとします。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220927190729.jpg)

TagFolderでは、これらのノートはこんなふうに表示されます。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220927191504.jpg)

Pouhon: 本当にフォルダみたいですね……タグによってノートがまとめられ、階層構造が作り出されました。

りんご.mdには『甘い』ディレクトリと『赤い』ディレクトリの両方からアクセスできるということですね。  
それ自体はすごく自然なアクセス経路だとは思いますが、逆に「経路が増えすぎる」という問題は発生しないんでしょうか？

きみのぶ: 対策としては、**タグをネストする**ことです。これはObsidianの基本機能ですね。

### ネストされたタグの扱い

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928110658.jpg)

きみのぶ: TagFolderは**ネストされたタグを尊重**します。わかりやすく言えば、このような構造として扱うということです。

-   赤い
    -   甘い

この場合は #赤い タグが上位であり、#甘い タグからはりんご.mdにはアクセスできません。  
その構造をより明確にするために、#赤い タグだけを付けたノートをもう1つ追加してみましょう。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928111659.jpg)

Pouhon: なるほど。ネストしているだけだから、 #甘い タグは**あくまで1つのタグだけれども、上位/下位という関係ができる**。  
りんご.mdの #甘い は #赤い の下位にしか存在しないタグだから、りんご.mdの #甘い タグは第一階層には出てこないと。

きみのぶ: これがTagFolderの基本的な仕様ですね。

## きっかけはTwitter

Pouhon: これは新感覚で、僕も初めて使った瞬間、正直驚きました。  
なかなかタグを「こう使おう」という発想に至らないと思うんですが、開発のきっかけはどんなところにあるんでしょうか？

きみのぶ: これは実は、Twitterのフォロワーさんにインスピレーションいただきました。 ずーっとうっすら考えていたものが、そのツイートで完全に可視化されて。

フォルダを捨てるためのベン図……**捨てなくてもベン図が表現できれば良い**の？ みたいな感じで、思いついてすぐ作りました。

Pouhon: よく言われている**コウモリ問題**にも通じる問題提起ですね。

![](https://pouhon.net/wp-content/uploads/2021/08/ss_20210819_011-1.png)

フォルダってのははっきりと「枠」が決まっていて、**とりあえずどちらにも入れておきたい**というニーズに答えるのが難しい。  
これをやろうとすると、エイリアスやショートカットといった**シンボリックリンク**を使う必要があって、それがかなり面倒だし美しくないんですよね。

タグをあたかもフォルダのように扱うと、簡単で柔軟な「タグ付け」でそれができてしまう。  
1つの『こうもり.md』で「獣の」こうもりと「鳥の」こうもり両方を表現できるし、「鳥ではない」となれば、そのときにタグを1つ削除すれば済むと。

きみのぶ: まさにそれです！ フォルダへの分類って**めちゃくちゃハイレベルな行為**だと思うんですよね。

### フォルダへの分類はハイレベルな行為

Pouhon: ハイレベルですよ……めちゃくちゃハイレベルで、僕は到底できる気がしません。  
できるとしたら仕事関係かな。いつの仕事で、誰が何を担当したかがわかれば良い、みたいな感じなら「時間軸」や「取引先」を基準にすればいいと思うんですけど、「知識」という幅広いものになるとかなりキツいですね。

タグは**付けたり外したりが自由にできる**分、ある意味「ダメもと」というか、とりあえずそこに付けておくことができるし、修正も簡単にできます。そういう意味では多くのユーザーにおすすめできるかもしれません。

きみのぶ: 「Macの[スマートフォルダ](https://support.apple.com/ja-jp/guide/mac-help/mchlp2804/mac)が欲しい」という願望と「フォルダからの脱却」というアイデアが悪魔合体した感じです。やってみたらできた、みたいなパターンでした。

Pouhon: 『女神転生』ですね。これについては機会があれば色々と語りたいところですが、**ほとんどの読者が置いてきぼり**にされてしまう可能性があるので、今は話を先に進めましょう。

## 製作者のタグの使い方

Pouhon: 僕も以前からTagFolderユーザーの1人なんですが、使い方としてはすごくシンプルで、「特定のタグが付いたデイリーノートを振り返る」ために使ってるんですね。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928201547.jpg)

通常のタグ一覧より少ない手順でアクセスできるのが、僕にとっては手放せないポイントです。なのでタグのネストはあまり積極的には使ってないんですが、きみのぶさんはどんなふうにタグをお使いですか？

### タグで階層化し、フォルダは使わない

きみのぶ: 僕は割とネストして階層化する方でして、情報は情報として「まとまった単位」になり次第タグをふっていきます。

一方でデイリーノートにはdaily/2022/20みたいなタグを付けてて、そこから先はアキネーターと同じですね。  
上から順に「開く」か「スキップする」で目当てのノートが見つかるようにしてます。 もちろん確実に単語がわかっていればそれを開きますが。

Pouhon: 「dailyの、2022年の、20周目」という感じでまとまっているということですね。つまり本当にフォルダとしてタグを使っている？  
実際のフォルダを使うことはありますか？

きみのぶ: 実際のフォルダは**デイリーノート程度しか使ってない**です。  
一時期、ブログをObsidianでやろうとして作成したフォルダもありますが、基本的には雑然と入ってますね(笑)

Pouhon: 割り切ってるなぁ……そこまで割り切れるパワフルさがTagFolderにはあるということですね。

### タグの命名規則と増殖防止策

Pouhon: タグの「命名規則」や付ける基準に関しては、一般的なイメージと変わらないですか？ 独自のルールがあったりします？ (「タグ増えすぎ問題」や「管理しきれない問題」に対するヒントなどあれば)

きみのぶ: タグの命名は、ちょっと前までは並び順のルールを決めていましたが、最近**ピン留めできる**ようになったのもあって、今は完全に自由につけています。  
タグ増えすぎ問題は逆の発想で、**既に存在するタグと同じ場所に入れるところから始めよう**、みたいな感じで、まず既存のタグを付けるようにしていますね。

Pouhon: サイドバーで個々のタグが一覧になっていると、そういう使い方には便利ですね。

## その他の機能

Pouhon: 使ってみると意外とシンプルなTagFolderですが、ちょっとした機能はそこかしこに隠れているということで、ここからはきみのぶさんから、TagFolderのその他の機能をいくつかご紹介していただくとしましょう。

### タグを右クリックしてノートを作成

きみのぶ: 最初に**新規ノート作成**ですね。  
TagFolderのペインでタグ名を右クリック → 「New note in here」を選択すると、そのタグが付いた状態のノートを新規作成できます。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928202913.jpg)

### TagScroll

きみのぶ: ノート数が表示されている数字部分をクリックすると、そのタグが付いたノートを全て**スクロールで閲覧できる**ようになりました。これがTagScrollです。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928203157.jpg)

Pouhon: 画像では若干わかりにくいかもしれませんが、ちょうど[Daily notes Viewer](https://github.com/Johnson0907/obsidian-daily-notes-viewer)みたいな感じですね。これは複数ノートの振り返りに便利そう。

### タグをピン留め

きみのぶ: 上の方でチラッと出てきましたが、**任意のタグをリストの上部に固定**するピン機能も搭載しています。  
使用するにはプラグインの設定 > 【Use pinning】をONにして、タグ名を右クリックしてください。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928203649.jpg)

Pouhon: その下の【Pin infomation file】というのは？

きみのぶ: Pin informationは**ピン留めの情報ファイル**です。LiveSyncやお好みのアプリで同期できるように、マークダウンファイルとして保存しています。このファイルを編集することで、ピン留めしたタグをグループ分けしたりもできますよ。

Pouhon: なるほど。マークダウンファイルで保存しておけば、設定フォルダの同期によらず、全ての端末で同じピン設定を使えると言うことですね。

### Virtual Tags

きみのぶ: 最後にVirtual Tagsです。これは設定 > 【Use virtual tags】をONにすると有効になります。

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928204645.jpg)

Pouhon: なんだか新しくタグが付与されましたが、これはどういう意味のタグなんでしょう？

![](https://pouhon.net/wp-content/uploads/2022/09/ss20220928204911.jpg)

きみのぶ: **ファイルの最終更新日**を参照して、それによって自動的にタグ付けしています。現在のところ、タグは全部で5種類ありますが、それぞれのタグが意味するところはGitHubのREADMEをご覧ください。

[

GitHub - vrtmrz/obsidian-tagfolder

Contributetovrtmrz/obsidian-tagfolderdevelopmentbycreatinganaccountonGitHub.





](https://github.com/vrtmrz/obsidian-tagfolder "GitHub - vrtmrz/obsidian-tagfolder")

Pouhon: こんな機能もあったのか……このVirtual TagsがMacのスマートフォルダ的な役割を果たしているとも言えますね。  
僕としてはこのタグが表す期間と、タグ自体の絵文字をカスタマイズできたら楽しいかなぁ。

きみのぶ: これまでにRecent Filesというプラグインも使っていたのですが、こういう時間とかメタな情報も何とか入らないかな、と思って実装した機能です。

期間のカスタマイズは必要かもしれないですね。今後のアップデートにご期待ください。

## 設定項目について

Pouhon: 特殊な機能としてはこんなところでしょうか。個人的にこのプラグインに関しては、あまり**細かく設定を変更する必要は無い**と感じているんですが、中にはサイドバーの表示に大きな影響を与える項目もあります。それが、

-   Merge redundant combinations
-   Do not treat nested tags as dedicated levels

の2つです。これについて解説をお願いしてよろしいでしょうか？

### Merge redundant combinations

きみのぶ: こちらは簡単に言うと、**重複している組み合わせを減らして表示**する機能です。  
例えば「#赤い」「#甘い」「#フルーツ」という3つのタグが付けられたノートがあった場合、サイドバーでは通常、下記のように展開されます。

-   赤い
    -   甘い
        -   フルーツ
    -   フルーツ
        -   甘い
-   甘い
    -   赤い
        -   フルーツ

多くなりすぎるのでいくつか省略しますが、**6種の組み合わせ全て**がディレクトリとして表示されるということですね。  
それを、可能であればこんな風に省略するオプションです。

-   赤い
    -   甘い
        -   フルーツ

他に同じタグが付いたノートが無い場合は「#赤い/甘い/フルーツ」と表示されます。まとめられない場合は限界までまとめて表示します。  
タグをどう付けているかによって、ONが良いのかOFFが良いのかが分かれるオプションかもしれません (僕はOFFに設定しています)

### Do not treat nested tags as dedicated levels

きみのぶ: これは**ネストされたタグを尊重するかどうか**の項目です。  
この設定がONのとき、記事の冒頭で示した例のように「#赤い/甘い」はその順序が構造として固定されるので、りんご.mdは「#甘い」タグからはアクセスできません。

OFFにすると「#赤い/甘い」は「#赤い」と「#甘い」に分離して解釈されるので、「#甘い」タグからもりんご.mdにアクセスできます。

## 今後について

Pouhon: ありがとうございます。おかげさまで、TagFolderを使い始める上で知っておきたいポイントが浮かび上がってきました。あとはもう慣れるだけですね。  
仕組みとしては「ノートに付けられたタグを階層化して、フォルダのように表示する」だけなので、ノートの内容を勝手に書き換えられるといったリスクはありません。なのでお気軽に使ってみていただければと思います。

「Self-hosted LiveSync」、「TagFolder」とかなり特徴的なプラグインをリリースされてきたきみのぶさんですが、この記事の締めとしてTagFolderの今後の方向性や、その他予定されている新たなプラグインなどの情報があれば。

きみのぶ: TagFolder はもう少し高速化したいなぁ……とは考えています。  
とはいえ、内部では案外ややこしい処理をしているので、腰を据えないと難しいかもです。既に一度失敗しているので(笑)

新しいプラグインは色々作っては公開せずに終わってますね。最近作りかけてるのは**読み上げプラグイン**なんですが、Windowsに読み上げエンジンを入れ損ねてしゅんとしてました。  
やっと時間もできてきたので、何かやりたいですね。

Pouhon: テキスト読み上げ機能を搭載したアプリもちらほら出てきてはいますが、プラグインでできるもんなんですか…… もしObsidianで読み上げできるとしたら、どんなユースケースが思い浮かびます？

きみのぶ: 僕は**英単語の発音チェック**に使いたくて作りました。読み上げも「やってみたらできた」ひとつですね。

Pouhon: 「チャレンジすることの大切さ」をお伝えいただきました。締めとして素晴らしいコメント、ありがとうございます。  
ということで今回のゲスト、vorotamoroz (ヴォロータ・モローズ) aka きみのぶさんでした！

また機会があれば、ぜひお越しください。お待ちしています。