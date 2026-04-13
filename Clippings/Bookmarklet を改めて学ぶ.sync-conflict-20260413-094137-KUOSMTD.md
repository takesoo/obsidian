---
category: "[[Clippings]]"
author: "[[defaultcfフォロー]]"
title: Bookmarklet を改めて学ぶ
source: https://zenn.dev/defaultcf/articles/learn-bookmarklet
clipped: 2023-09-19
published: 
topics: 
tags:
  - clippings
  - bookmarklet
---

業務でもプライベートでも、Bookmarklet 無しには生きていけない私です。  
ちなみに最近私が書いた便利そうなコードは、後述する[コチラ](#%E6%9C%80%E8%BF%91%E6%9B%B8%E3%81%84%E3%81%9F%E4%BE%BF%E5%88%A9%E3%81%9D%E3%81%86%E3%81%AA-bookmarklet)です。

ところで Bookmarklet ってなんなんでしょう...？思えば Bookmarklet の公式リファレンスみたいなものを見たことがありませんでした。起源は？どうやって実行できている？調べてみました。  
間違ってたらコメント欄で教えてください。

## 起源

調べてみると下記サイトが随分古くからあるようで参考になりそうです。このサイトが Bookmarklet を命名したのかな？

このページによると、Bookmarklet のアイデアは Netscape JavaScript Guide から来ている、とあります。  
[http://developer.netscape.com/docs/manuals/communicator/jsguide/misc.htm](http://developer.netscape.com/docs/manuals/communicator/jsguide/misc.htm) （developer.netscape.com 自体がもう生きていないようです。ちなみに netscape.com は www.aol.com にリダイレクトしますね。）  
Internet Archive にページが保管されていました。2002年頃まで配信されていたようです。

Netscape Navigator 4.0 にツールバーが導入され、ここで Bookmark に JavaScript が書けるようになったとあります。  
4.0 は1997年6月リリースのようで[\[1\]](#fn-54aa-1)、そこから後身の Firefox に引き継がれ、Internet Exploler や後の Google Chrome にも導入され、という流れのようです。  
（他ブラウザがどのように追従するようになったかまでは、調べきれませんでした...）

あくまで Bookmark の中で JavaScript を動作させるというもので、特に標準化されているものでも無さそうです。  
UserScript や Browser Extension より手軽に書いて実行できるので、第三の選択肢として今後も無くならないでほしいですね...

## どうやって実行できている？

Bookmarklet は必ず `javascript:` から始まります。  
先程の Netscape のページでもしれっと `javascript:` から始まる旨が書かれていましたが、そもそもこれはなんなのでしょう？  
ググると次の記事がヒットしました。 ウェブブラウザやユーザーに、HTML タグの attribute 内の文字列が JavaScript であることを明示するためのプレフィクスです。（そういえば確かにそうだ...）  
a タグの href では、リンクをクリックした際の挙動を JavaScript で宣言できます。なお技術的に可能なだけでこの書き方は推奨されません [\[2\]](#fn-54aa-2)。

Bookmarklet ではまさにこの仕組みを利用して、ブックマークを開く際にこの文字列が単なる URL ではなく JavaScript であることをウェブブラウザに伝えます。

そして Bookmarklet の実行は、アクティブになっているタブのウェブページ内で行われます。  
グローバルスコープで実行されることに留意すべきです。既存のコードに悪影響を与えるのを防ぐため、Bookmarklet を無名関数で包むと良いでしょう。

## トランスパイル

ブックマークに登録するという性質上、Bookmarklet は1行で書かなければなりません。  
今の Firefox と Google Chrome では複数行の JavaScript をブックマークとして登録しようとすると、自動的に改行コードが空白に置換されていそうですが、Internet Exploler などは自動的に置換されず予め1行にしておかなければならないようでした[\[3\]](#fn-54aa-3)。また意図的に改行を使いたい場合は工夫が必要です。

また、第三者に Bookmarklet を配布する場合、ブログなどで次のように a タグの中に入れて配布する場合があります。

```
<a href="javascript:(function(){a:alart('hello')}())">alart</a>
```

この場合、ただコードを貼り付ければ良いわけではありません。次のような処理をしておかないと a タグがきちんと表示されません。

-   ダブルクオーテーションなどは URI エンコードする必要がある
-   改行は空白に置換するか、文字列の中なら改行文字に置換する必要がある

手作業でやるのは非効率なので、自動でやりましょう。  
そのためのツールなどがネット上にいくつかあります。パッと調べたところ使っている人が多そうなのはコチラでしょうか。

URI エンコードして、全体を無名関数で包む変更をしてくれています。  
無名関数で包まなければ、Bookmarklet は実行時のウェブページのグローバルスコープで影響を与えてしまうためです。

これを全てやってくれる CLI ツールがあったらな、と思ったらありました。

次のような感じで実行できます。

```
cat copytitle.js | npx bookmarkletter
```

ただ古いツールで、ECMAScript 2015 以降のコードを食わせるとエラーになりました。依存しているパッケージのバージョンが古く、ECMAScript 2015 以降に対応していないためです。  
手元でいくつかの依存パッケージをアップデートしたら動くようになりました。  
今度 Pull Request を送ってみようかと思います。

## 最近書いた便利そうな Bookmarklet

最後に、最近私が書いて楽だなって思ってるコードはコレです。

```
javascript:(async () => {
  if (window.self !== window.top) return;
  const a = document.createElement("a");
  a.href = location.href;
  a.innerText = document.title;
  const image = document.createElement("img");
  image.src = document.querySelector("meta[property='og:image']").content;

  const text = `${document.title} ${location.href} ${image.src}`;
  const container = document.createElement("div");
  container.appendChild(a);
  container.appendChild(image);

  const data = [new ClipboardItem({
    "text/html": new Blob([container.outerHTML], { type: "text/html" }),
    "text/plain": new Blob([text], { type: "text/plain" }),
  })];
  await navigator.clipboard.write(data);
  alert("コピーした");
})()
```

今いるウェブページのタイトルと URL をコピーして、「リンク付きのテキストとOGP画像」と「タイトル・URL が並んだプレーンなテキスト」の両方をクリップボードに保存する Bookmarklet です。  
こうすると、リッチなテキストエディタ（Confluence や Slack など）に貼り付ければリンク付きテキストと OGP 画像を貼り付けられ、Vim や Zoom のコメント欄などに貼り付ければプレーンなテキストが貼り付けられます。  
ちなみにリッチなテキストエディタでも `ctrl + shift + v` で貼り付ければ、プレーンなテキストの方を貼り付けられます。

他の人にリンクを共有する時に便利かと思うので、是非使ってみてください。

脚注

1.  Netscape (web browser) - Wikipedia [https://en.wikipedia.org/wiki/Netscape\_(web\_browser)#Release\_history](https://en.wikipedia.org/wiki/Netscape_(web_browser)#Release_history) [↩︎](#fnref-54aa-1)
    
2.  予期せぬ動作を引き起こしたり、アクセシビリティ面でもよろしくないためです。代わりに button を使用すべきです。詳しくはこちら [<a>: アンカー要素 - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element/a#onclick_%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88) [↩︎](#fnref-54aa-2)
    
3.  ブラウザのアドレスバーからJavaScriptのコードを実行する方法 (JavaScript疑似プロトコル) [https://so-zou.jp/web-app/tech/programming/javascript/sample/javascript-scheme.htm](https://so-zou.jp/web-app/tech/programming/javascript/sample/javascript-scheme.htm) [↩︎](#fnref-54aa-3)