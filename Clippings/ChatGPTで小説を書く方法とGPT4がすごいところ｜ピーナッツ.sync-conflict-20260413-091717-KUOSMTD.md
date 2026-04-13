---
category: "[[Clippings]]"
author: "[[note（ノート）]]"
title: "ChatGPTで小説を書く方法とGPT4がすごいところ｜ピーナッツ"
source: https://note.com/1za5ij/n/n8add9bcc280e
clipped: 2023-11-10
published: 2023-03-18
topics: 
tags: [clippings]
---

**なんの役にも立たないゴミをChatGPTでつくったことはあるか？**

俺はある。

これらは筆者がChatGPTでつくった小説である。どちらもかなり長い。忙しい読者のためにあらすじだけ紹介したい。

**「青木の転職」**はGPT3.5で作成した。  
経営学修士号を取得しながらもメーカーに就職した青木が、田舎の工場での単純労働に疲弊し、きらきらしたベンチャー企業に就職するもソッコーで出戻りする物語だ。

**「ジョージ(60)の起業」**はGPT4で作成した。  
JTCを定年退職した御年60の元技術者ジョージが、JTCが世間から見下されていることに憤怒し、「JTCをバカにするな」という少年のような思いをもって特許事務所を起業する物語だ。

本稿は、これら二編の小説をChatGPTで書いてみた筆者なりの手法と所感をまとめたものである。

**本稿の全体像を先取りしよう。  
ChatGPTを用いた小説作成の方法論を前半部分で解説し、後半ではGPT3.5からGPT4の進化、とりわけ小説をつくるにあたっての性能向上について筆者が抱いた所感を述べる。小説をつくりたい方は前半から読んでほしいが、チャットAIにおける技術としての考察に興味がある方は後半だけ読めばよい。**

ではさっそく「ChatGPTによる小説作成」について具体的なプロンプトを交えて解説・・・といきたいところだが、最後に一つだけ重要な注意点がある。

**GPT3.5とGPT4で「小説作成」という同一のタスクにトライした筆者は、このような作成手法なるいわば「プロンプトのノウハウ」がとてつもないスピードで陳腐化することを確信している。**

なので、ここに書く方法論も全くもって使えないものになるだろう。そこを承知いただきたい。

二編だけ書いて「ノウハウ」のようなイキった筆致を世間にさらすのは正直いかがなものかと躊躇したのだが、これをモヤモヤと考えてるうちに技術が進化し、いま自分にある考えが問答無用でゴミになる未来がくっきりと見えてしまった。「昔はこんなことを考えていたよ」を残す意味でも、書いてしまおうという結論に至った。

前置きが長くなった。それではまず「ChatGPTで小説を書く方法」を解説していく。

## ChatGPTで小説を書く方法

これはChatGPTに限った話ではないのだが、生成AIのプロンプトは森から木、木から葉を指示するのが効果的と筆者は考えている(この考えもおそらくすぐに使えないものになるだろう)。

小説においてもこれが原則となる。大まかな流れとして、小説をChatGPTで書くプロンプトは以下の6ステップから構成される。

-   小説を書いてほしいことを伝える
    
-   メインテーマ、主人公の人物像をインプットする
    
-   全体のプロットと章構成を決めてもらう
    
-   登場人物をつくってもらう
    
-   その他の限定事項を伝える
    
-   各章を書いてもらう
    

それでは各ステップについて、具体的なプロンプトを提示しながら解説しよう。

### 小説を書いてほしいことを伝える

まずはChatGPTに「ゴーストライター」となってもらうことをインプットしよう。プロンプトは以下。

> PROMPT : From now on I want you to pretend you are the author of a novel. You will be a ghostwriter for a novel I am writing. Acknowledge Yes or No.
> 
> 今からあなたは小説の作者。あなたは私が書く小説のゴーストライターになるのです。いいね？

[![画像](https://assets.st-note.com/img/1679092795774-weJL2BX88G.png?width=800)](https://assets.st-note.com/img/1679092795774-weJL2BX88G.png?width=2000&height=2000&fit=bounds&quality=85)

### メインテーマと主人公の人物像をインプットする

次に小説で描くテーマと主人公の人物像を設定する。「ジョージの起業」ならば、描くのは一人の男性が起業する物語であり、JTCが世間から嘲笑されることへのジョージの怒りと劣等感がメインテーマとなる。

> PROMPT : Firstly, I'm going to provide main theme of this novel. This novel is about a man who starts his own business. Main theme of this novel is his anger and sense of inferiority.
> 
> この小説のメインテーマを提示します。この小説はビジネスを始める男の話です。この小説の主題は彼の怒りと劣等感です。

[![画像](https://assets.st-note.com/img/1679092821261-eYPYKOQVw8.png?width=800)](https://assets.st-note.com/img/1679092821261-eYPYKOQVw8.png?width=2000&height=2000&fit=bounds&quality=85)

次に、主人公の人物像を与える。ジョージは60歳の男性、元JTCのエンジニア。さらに今回は、彼が起業するのは特許事務所であることも決めていたので、この段階でその情報も伝える。

> **PROMPT :**  
> Yes. I'm going to provide information about the protagonist and the company the protagonist is starting.
> 
> protagonist :  
> 60-year-old male. He is a former engineer who retired from JTC, a large manufacturing company.
> 
> Company started by the protagonist :  
> Patent firm. The source of revenue is royalties from patents. The employees are protagonist and four former engineers who retired from JTC.
> 
> 主人公と主人公が起業する会社の情報を提供します  
> 主人公 :60歳の男性。大手製造業のJTCを定年退職した元エンジニア  
> 主人公が立ち上げた会社 :特許事務所。収入源は特許のロイヤリティ。社員は主人公とJTCを退職した元エンジニア4名。

[![画像](https://assets.st-note.com/img/1679092846988-Ul9MVQYTwa.png?width=800)](https://assets.st-note.com/img/1679092846988-Ul9MVQYTwa.png?width=2000&height=2000&fit=bounds&quality=85)

そして主人公がなぜ起業するのか？を伝える。より一般的にいうと「物語の背景」である。

「世間がJTCを時代遅れとけなすことにジョージはキレている、彼の起業は世間が間違っていることを証明するためなんだ」と。

> PROMPT：  
> Here is protagonist's motivation for starting his own business:
> 
> JTC was derided by the public as outdated. Protagonist was angry and felt inferior that the company he had devoted himself to was being ridiculed. By making his own business successful, he wanted to prove that he and JTC were not wrong and that the public was wrong. Four colleagues were sympathised with the protagonist's desire to 'look back at the public'.
> 
> 主人公の起業の動機：  
> JTCは世間から時代遅れと揶揄されていた。主人公は、自分が心血を注いできた会社が揶揄されることに怒りと劣等感を抱いていた。自分のビジネスを成功させることで、自分とJTCは間違っていない、世間は間違っていると証明したかったのだ。そして「世間を見返したい」という主人公の思いに、4人の同僚が共感した。

[![画像](https://assets.st-note.com/img/1679092869015-gylbSLHxSp.png?width=800)](https://assets.st-note.com/img/1679092869015-gylbSLHxSp.png?width=2000&height=2000&fit=bounds&quality=85)

これで小説の屋台骨が完成した。

### 全体のプロットと章構成を決めてもらう

小説全体のプロット(物語の構成)の作成をChatGPTに依頼する。

ここで一つ、GPT3.5からGPT4への進化に関するトピックを紹介しよう。GPT4では、以下のように「プロットを提示して」というと章構成もあわせて提案してくれる。

> PROMPT : Could you provide a story plot for this novel?
> 
> この小説のプロットを提示して

[![画像](https://assets.st-note.com/img/1679092890591-fVSD6S4A1B.png?width=800)](https://assets.st-note.com/img/1679092890591-fVSD6S4A1B.png?width=2000&height=2000&fit=bounds&quality=85)

しかしGPT3.5では章構成まで提案してくれない。なのでGPT3.5でやる人は、プロットを書いてもらってから、そのあとに章構成を決めてもらう必要がある。

[![画像](https://assets.st-note.com/img/1679092897915-ucH3g1oEjO.png?width=800)](https://assets.st-note.com/img/1679092897915-ucH3g1oEjO.png?width=2000&height=2000&fit=bounds&quality=85)

プロット提示してくれる？

[![画像](https://assets.st-note.com/img/1679092908423-oVnAURWwzs.png?width=800)](https://assets.st-note.com/img/1679092908423-oVnAURWwzs.png?width=2000&height=2000&fit=bounds&quality=85)

チャプターのリストを作ってくれる？

### 登場人物をつくってもらう

次に以下のプロンプトを打ち込み、登場人物をセットしてもらおう。自分の構想にないユニークなキャラクターを作り込んでくれるぞ。個人的にはここが一番たのしい。

> Next, provide a list of main and supporting characters. Please include their full names , ages and personalities.
> 
> 主役と脇役のリストを用意してください。フルネーム、年齢、性格を記載してください。

[![画像](https://assets.st-note.com/img/1679093881886-3QvRw4Ry95.png?width=800)](https://assets.st-note.com/img/1679093881886-3QvRw4Ry95.png?width=2000&height=2000&fit=bounds&quality=85)

### その他の限定事項を伝える

会話をふんだんに入れてね、など特別指定したいテイストがあればここで教えておこう。とくになければ飛ばしてもらって構わない。ここまでで準備は完了。

> I'd like to include many dialogues to give the novel a sense of realism.
> 
> 台詞を多く入れて、小説の臨場感を出したいですね。

### 各章を書いてもらう

あとは実際に書いてもらう。いい感じだったら、「次もお願い」とすればどんどん書いてくれる。プロンプトは以下のようなものを使えばよい。

> First of all, could you actually write chapter1?
> 
> Let's move on to the next chapter.

## GPT4とGPT3.5との違い

筆者は、ChatGPTでくだらない小説を二編書いた。「青木の転職物語」はGPT3.5、「ジョージの起業」はGPT4で書いた。

小説作成という観点で、あくまでAIド素人の筆者が感じた「言語モデルの進化」をいくつか紹介したい。

### 文学的な表現技法が豊か

GPT3.5とGPT4における「ジョージの起業」の書き出しを比較してみよう。

> John sat at home, staring blankly at the television. The news segment on JTC, the company he had devoted his entire career to, was playing on the screen. The anchors spoke about how the company had become outdated, how it was struggling to keep up with the times, and how it was being criticized by the public.
> 
> ジョンは自宅に座り、ぼんやりとテレビを見つめていた。画面には、彼がこれまでキャリアを捧げてきたJTCのニュース番組が流れていた。キャスターは、会社がいかに時代遅れになったか、いかに時代についていけず、いかに世間から批判されているかを語っていた。

GPT3.5の書き出し

> Beneath the overcast skies of the industrial city, George Thompson, a 60-year-old retired engineer, took a long, contemplative sip from his steaming cup of coffee. The aroma of roasted beans and the white noise from the bustling cafe mingled with his thoughts,
> 
> 工業都市の曇り空の下、60歳の定年退職したエンジニア、ジョージ・トンプソンは、湯気の立つコーヒーカップにゆっくりと口をつけた。焙煎した豆の香りと賑やかなカフェの雑音が、彼の思考と混ざり合う。

GPT4の書き出し

どうだろう。  
表現技法が格段に向上していないだろうか？「焙煎した豆の香りと賑やかなカフェの雑音が、彼の思考と混ざり合う」である。筆者にはとてもではないがマネできない。

### 事前情報がいらない

実をいうと、「ジョージの起業」の作成に筆者はかなりてこずった。小説の設定がかなりマニアックだったからなのか、GPT3.5ではマシなものが書けなかった。なのでGPT4が出たとき、筆者はすがる気持ちでこれに飛びついた。そして驚愕した。

前半部分を読んでない人のために再掲する。ChatGPTで小説をつくるには以下の手順を踏む。

-   小説を書いてほしいことを伝える
    
-   メインテーマと主人公の人物像をインプットする
    
-   全体のプロットと章構成を決めてもらう
    
-   登場人物をつくってもらう
    
-   その他の限定事項を伝える
    
-   各章を書いてもらう
    

上記における「メインテーマと主人公の人物像をインプット」での、GPT3.5とのやりとりが以下になる。「主人公は60歳で、JTCが世間から見下されていることにキレている。彼は自分の事業を成功させることで、世間が間違っているのを証明したい。」という筆者のインプットに対して「なるほど」という感じの返答をChatGPTがしている。  

[![画像](https://assets.st-note.com/img/1679094008229-iSr9sxiFJg.png?width=800)](https://assets.st-note.com/img/1679094008229-iSr9sxiFJg.png?width=2000&height=2000&fit=bounds&quality=85)

GPT3.5では、ここから小説全体と章の構成を決め、登場人物を決めて第１章を書いてもらう。

一方、GPT4に「主人公の人物像」をインプットするとこう返ってくる。与えたプロンプトは以下(日本語訳のみ)。

> 主人公と主人公が起業する会社の情報は以下です  
> 主人公：60歳の男性。大手製造業のJTCを定年退職した元エンジニア  
> 主人公が立ち上げた会社：特許事務所。収入源は特許のロイヤリティ。社員は主人公とJTCを退職した元エンジニア4名。

[![画像](https://assets.st-note.com/img/1679094034015-ZB80zUD18P.png?width=800)](https://assets.st-note.com/img/1679094034015-ZB80zUD18P.png?width=2000&height=2000&fit=bounds&quality=85)

わかるだろうか？  
**なんと主人公を「60歳でJTCを定年退職した元エンジニア」と設定しただけで、ChatGPTがいきなり第1章を書き始めたのだ。**

「JTCが世間から嘲笑されていることに主人公がキレてる」という設定は筆者にとってマストだったので、「ちょっと待て」と制したが、これは本当に衝撃的だった。

「青木の転職」と「ジョージの起業」とは別の題材も実はいくつかトライしているが(失敗してる)、GPT3.5がこんな早い段階で第１章を書き始めたことは今までただの１度もなかった。

とんでもない進化を感じずにはいられなかったのだ。

### 登場人物がぶれない

これも劇的に進化した。GPT3.5では、登場人物がとにかくぶれる。

男気あふれる上司が後半で急に女になったり、なんか知らないけど60歳手前の同僚が若返って、若気の至りで会社を危機に陥れたりする。

一回そうなってしまうと、軌道修正がとても難しい。「女じゃないでしょ。もう一回書いてよ」というと、全体のプロットと整合性がとれないものを書いてきたりする。

とくに「ジョージの起業」については、筆者はなんどもこういったカオスなキャラ変で涙を飲んできた。

しかしながらGPT4ではこれが全くと言っていいほどない。事前につくった登場人物、そして前の章で描いた人物像が驚くほどぶれない。

おかげで、半ばあきらめかけていた「ジョージの起業」がたった1回のやりとりで日の目を見ることになった。

ChatGPTの進化は止まらない。これからも筆者はテクノロジーを利用してクソな小説を書いていく。

## 参考