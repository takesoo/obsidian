---
Tags:
  - Next.js
  - React
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
[https://miro.com/app/board/uXjVOJaiLZw=/?invite_link_id=600280404333](https://miro.com/app/board/uXjVOJaiLZw=/?invite_link_id=600280404333)

# React Tutorial

[

チュートリアル：React の導入 - React

このチュートリアルは React の事前知識ゼロでも読み進められます。 このチュートリアルでは小さなゲームを作成します。 自分はゲームを作りたいのではないから、と飛ばしたくなるかもしれませんが、是非目を通してみてください。 このチュートリアルで学ぶ技法はどのような React のアプリにおいても基本的なものであり、マスターすることで React への深い理解が得られます。 ヒント このチュートリアルは実際に手を動かして学びたい人向けに構成されています。コンセプトを順番に学んでいきたい人は 一段階ずつ学べるガイド を参照してください。このチュートリアルとガイドは互いに相補的なものです。 このチュートリアルは複数のセクションに分割されています。 このチュートリアルから価値を得るために全セクションを一度に終わらせる必要はありません。セクション 1 つや 2 つ分でも構いませんので、できるところまで進めましょう。 このチュートリアルでは、インタラクティブな三目並べゲーム (tic-tac-toe) の作り方をお見せします。 最終的な結果をここで確認することができます：最終結果 。まだコードが理解できなくても、あるいはよく知らない構文があっても、心配は要りません。このチュートリアルの目的は、React とその構文について学ぶお手伝いをすることです。 チュートリアルを進める前に三目並べゲームで遊んでみることをお勧めします。いくつか機能がありますが、ひとつには、盤面の右側に番号付きリストがあることに気づかれるでしょう。このリストにはゲーム内で起きたすべての着手 (move) のリストが表示され、ゲームが進むにつれて更新されていきます。 どんな物か分かったら三目並べゲームを閉じて構いません。もっと小さな雛形から始めましょう。次のステップはゲームの構築ができるようにするための環境のセットアップ作業です。 HTML と JavaScript に多少慣れていることを想定していますが、他のプログラミング言語を使ってきた人でも進めていくことはできるはずです。また、関数、オブジェクト、配列、あるいは（相対的には重要ではありませんが）クラスといったプログラミングにおける概念について、馴染みがあることを想定しています。 JavaScript を復習する必要がある場合は、 このガイドを読むことをお勧めします。また ES6 という JavaScript の最近のバージョンからいくつかの機能を使用していることにも注意してください。このチュートリアルでは、 アロー関数、 クラス、 および ステートメントを使用しています。 Babel REPL を使って ES6 のコードがどのようにコンパイルされるのか確認することができます。

![](https://ja.reactjs.org/favicon-32x32.png?v=f4d46f030265b4c48a05c999b8d93791)https://ja.reactjs.org/tutorial/tutorial.html

![](https://reactjs.org/logo-og.png)](https://ja.reactjs.org/tutorial/tutorial.html)

reactのコンポーネント特性を理解

コンポーネントをオブジェクトと見做してシーケンス図で構成を考えられる？

![](https://drive-thirdparty.googleusercontent.com/64/type/application/vnd.jgraph.mxfile)[ReactTutorialSeaquence](https://drive.google.com/file/d/1KMB8g1htepggLUU9h5c6hZHBf-3buOOc/view?usp=drivesdk)  
[https://drive.google.com/file/d/1KMB8g1htepggLUU9h5c6hZHBf-3buOOc/view?usp=drivesdk](https://drive.google.com/file/d/1KMB8g1htepggLUU9h5c6hZHBf-3buOOc/view?usp=drivesdk)

## refとはなにか

AコンポーネントからBコンポーネントを操作する時にrefを渡す  
例えばボタンをクリックしたらinputの中身がリセットされるなど  
ただしコンポーネント間の依存が増すので推奨されず、refを使う前にpropsなどの構成を見直すべき

[https://miro.com/app/board/uXjVOJaiLZw=/?moveToWidget=3458764521332577680&cot=14](https://miro.com/app/board/uXjVOJaiLZw=/?moveToWidget=3458764521332577680&cot=14)

# React + TypeScript

```
import React from "react" // React関係の型はimport React from ‘react’でインポートする/* ---------- 親コンポーネント（propsを渡す側） ---------- */const Parent: React.VFC = () => { // React.VFCは関数コンポーネント型を意味する(Void Function Component)  const [inputText, setInputText] = useState("") // stateの型は基本的に型推論に任せてok  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => { // eventオブジェクトの方はvscodeの型推論に頼る    setInputText(event.target.value)  }  return (    <div>      <h1>{inputText}</h1>      <InputTextForm inputText={inputText} handleChange={handleChange} />    </div>  )}/* ---------- 子コンポーネント（propsを受け取る側） ---------- *//*  propsの型を定義してReact.VFC<P>ジェネリクスに渡して使用する	ちなみに、childrenの型はReact.ReactNode型*/type Props = {  inputText: string  handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void}/* 	const InputTextForm: React.VFC = (props: Props) と同じ	ちなみにpropsを分割代入するには ({inputText, handleChange}) とする*/const InputTextForm: React.VFC<Props> = (props) => {  return (    <div>      <input        type="text"        value={props.inputText}        onChange={props.handleChange}      />    </div>  )}
```

[

【初心者】React × TypeScript 基本の型定義

ここ最近TypeScriptの学習をしていまして、その学習記録をZennに投稿し続けていました。 その中で、TypeScriptの基礎学習の最後として投稿した以下の記事では、TypeScriptを用いてReact開発をする際に最低限必要となるであろうTypeScriptの型について簡単にまとめました。 先述の記事を書いている際、TypeScriptを用いたReactの基本的な型定義について網羅的にまとめている記事がまだまだ多くないように感じたため、今回「 React × TypeScriptの基本の型定義 」について改めてまとめ直してみることにしました。 TypeScriptの基礎学習を終え、これからTypeScriptを利用してReactやNext.jsでの開発をしてみようという方の参考になれば幸いです。 そこそこ長くなってしまったため、適宜必要な部分のみ参考にしていただくのがいいかもしれません。 TypeScriptを学習し始めて2週間程度の知識なので、間違いや修正点、追加したほうがいいことなどあれば是非是非コメントして頂けると嬉しいです。 今回扱わないこと この記事では、以下のことについては扱いません。 クラスコンポーネントは扱いません。全て 関数コンポーネント での記載となっています。 関数宣言（ function(){} ）を用いた記法は扱いません。全て アロー関数（ () => {} ）で表記しています。 TypeScriptの基礎的な型定義については扱いません。こちらは適宜学習をお願いします。おすすめはトラハックさんのこちらの動画シリーズです。 Reactの基本的な用語の解説は扱いません。Reactがまだ曖昧な方はこちらも適宜学習をお願いします。おすすめはじゃけぇさんのこちらのUdemy講座です。 この記事を読む上で最低限必要となる React と TypeScript の「基礎」って一体どこまで？ 上で「ReactやTypeScriptの基礎的な内容は扱いません」と注意書きを入れていますが、「じゃあこの記事を読む上で最低限どこまで知っていればいいんだ」という声が聞こえてきそうなので、以下にその目安を記しておきます。是非参考にしてみてください。 この記事を読み進める上での最低限必要な「基礎理解」とは以下の様な内容を指しています。 この記事が求める「 React 」の基礎理解 この記事が求める「 TypeScript 」の基礎理解 上記の挙げたことで分からないことがある場合は、ぜひ上の赤枠の中に載せてある動画などを用いて React や TypeScript の基礎学習から始めてみてください😌 とてもわかりやすいのでおすすめです！ 環境 TypeScriptを使用してのReact(Next.js)開発で、最低限知っておいた方がいいであろう型定義について解説していきます。 React用の型をインポートする際は、以下の様に書きます。 また、以下の様に import type { 特定の型 }と書くことで、「 型情報のみのインポート」をすることができます。より詳しく知りたい方は こちら の記事が参考になります。 個人的には import typeを使用した「型のみのインポート」の方がシンプルで好きです。 ですが、この記事では分かりやすさを重視して、見慣れている方が多いであろう import React from "react"のタイプで統一します。 省略したい方は適宜 React.VFCなどの React.

![](https://zenn.dev/images/logo-transparent.png)https://zenn.dev/ogakuzuko/articles/react-typescript-for-beginner

![](https://res.cloudinary.com/zenn/image/upload/s--QZQ40OtX--/co_rgb:222%2Cg_south_west%2Cl_text:notosansjp-medium.otf_37_bold:kou%2Cx_203%2Cy_98/c_fit%2Cco_rgb:222%2Cg_north_west%2Cl_text:notosansjp-medium.otf_70_bold:%25E3%2580%2590%25E5%2588%259D%25E5%25BF%2583%25E8%2580%2585%25E3%2580%2591React%2520%25C3%2597%2520TypeScript%2520%25E5%259F%25BA%25E6%259C%25AC%25E3%2581%25AE%25E5%259E%258B%25E5%25AE%259A%25E7%25BE%25A9%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9yZXMuY2xvdWRpbmFyeS5jb20vemVubi9pbWFnZS9mZXRjaC9zLS1GeHV2NV8xdy0tL2NfbGltaXQlMkNmX2F1dG8lMkNmbF9wcm9ncmVzc2l2ZSUyQ3FfYXV0byUyQ3dfNzAvaHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzY2YjVhZjU0OTkuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_72/v1627274783/default/og-base_z4sxah.png)](https://zenn.dev/ogakuzuko/articles/react-typescript-for-beginner)

  

---

# Next.js

Next.jsはReactの実行に必要なWebpackやBabelやらなんやらが全てパッケージされたフレームワーク

  

- Next.jsのしくみについて
    
    [
    
    Learn | Next.js
    
    Before you learn more advanced Next.js features, it would be helpful to understand the basics of how Next.js works. At the beginning of this course, we talked about how React is relatively unopinionated about how you build and structure your applications - there are multiple ways to build applications with React.
    
    ![](https://nextjs.org/static/favicon/favicon.ico)https://nextjs.org/learn/foundations/how-nextjs-works
    
    ![](https://nextjs.org/static/twitter-cards/learn-foundations.png)](https://nextjs.org/learn/foundations/how-nextjs-works)
    

  

## Tutorial

[

Learn | Next.js

To build a complete web application with React from scratch, there are many important details you need to consider: Code has to be bundled using a bundler like webpack and transformed using a compiler like Babel. You need to do production optimizations such as code splitting.

![](https://nextjs.org/static/favicon/favicon.ico)https://nextjs.org/learn/basics/create-nextjs-app

![](https://nextjs.org/static/twitter-cards/learn-twitter.png)](https://nextjs.org/learn/basics/create-nextjs-app)

  

routingはpages以下のディレクトリ構成で決まる

画像などの静的ファイルはpublic/ 以下に配置

その他のディレクトリ構成は自由だがcomponents/ にコンポーネント、styles/ にglobal.cssを置くのが一般的

  

---

  

# ReactQuery

[

React Query

Instead of writing reducers, caching logic, timers, retry logic, complex async/await scripting (I could keep going...), you literally write a tiny fraction of the code you normally would. You will be surprised at how little code you're writing or how much code you're deleting when you use React Query.

![](https://react-query.tanstack.com/_next/static/images/favicon-eed8346421218b24d8fd0fd55c2f9e35.png)https://react-query.tanstack.com/

![](https://react-query.tanstack.com/_next/static/images/react-query-og-bc3e2663a884437e074dc018c8f4e59f.png)](https://react-query.tanstack.com/)

[

初めてReact Queryを使う人のため設定方法と動作の理解 | アールエフェクト

React QueryはReactのData Fetchingライブラリでサーバからデータを取得する際に利用することができます。React Queryのドキュメントには"React Query makes fetching, caching, synchronizing and updating server state in React App a breeze"で記載されています。説明からサーバからデータを取得したり取得したデータをキャッシュしたりするためのライブラリであることはわかりますが実際にどのように動作するのか知りたいという人も多いかと思います。本文書ではReact環境を構築しuseQueryが実際にどのような機能を持っているのかuseState, useEffectの使い方を知っているだけのReactのビギナーレベルの人でも理解できようにできるだけ簡単な例を利用して説明しています。 useQueryの動作確認を行うためにReactのプロジェクトの作成を行います。npx crearte-react-appコマンドを実行します。プロジェクトの名前には任意の名前をつけてください。 % npx create-react-app react-userquery-test Reactプロジェクトが作成後、通常の方法でのデータ取得の方法確認し、その後useQueryを利用したデータの取得方法を確認していきます。 srcフォルダにcomponentsフォルダを作成しその下にUser.jsファイルを作成してください。 fetch関数を利用してデータを取得しますが、そのためには外部にデータリソースが必要となります。ここでは JSONPlaceholder を利用させてもらいます。下記のURLにアクセスすると10名分のユーザ情報を取得することができます。ブラウザからも取得が可能なのでどのようなデータが取得できるか確認してみてください。 https://jsonplaceholder.typicode.com/users User.jsファイルではReact HookのuseEffectを利用してマウント時にfetch関数を実行しJSONPlaceholiderからユーザデータを取得し、取得後はReact HookのuseStateを利用して取得したデータを保存します。その後はmap関数で展開を行うユーザ一覧をブラウザ上に表示させています。 import { useState, useEffect } from 'react'; const fetchUsers = async () => { const res

![](https://reffect.co.jp/wp/favicon.ico)https://reffect.co.jp/react/react-use-query

![](https://reffect.co.jp/wp-content/uploads/2021/06/react_query.png)](https://reffect.co.jp/react/react-use-query)

[

React Query の素振り

React Query はしばしば、React 用のデータフェッチングのためのライブラリだと言われるが、実際はフェッチングだけでなく、キャッシング及びサーバとの同期も行ってくれるライブラリだ。 React 自体には、コンポーネントからデータフェッチしたり更新するための仕組みが提供されていないため、結局開発者それぞれが、独創的な方法を実施することになる(React Hooks ...

![](https://zenn.dev/images/logo-transparent.png)https://zenn.dev/sa2knight/scraps/46924961062c08

![](https://storage.googleapis.com/zenn-user-upload/avatar/845fa75ba8.jpeg)](https://zenn.dev/sa2knight/scraps/46924961062c08)

useQueryでfetch

useMutateでcreate, update, deleteする

面倒なハンドリングをよしなにやってくれる

firebaseで試してみたい

  

  

https://github.com/takesoo/NextjsTutorial

[

GitHub - takesoo/NextjsTutorial: Nextjsのtutorial学習リポジトリ

You can't perform that action at this time. You signed in with another tab or window. You signed out in another tab or window. Reload to refresh your session. Reload to refresh your session.

![](https://github.com/favicon.ico)https://github.com/takesoo/NextjsTutorial

![](https://opengraph.githubassets.com/8a1572dffd8da70dfd231ec3695cedf4ffa4afed41b8c14a54c3566aa4fba92a/takesoo/NextjsTutorial)](https://github.com/takesoo/NextjsTutorial)