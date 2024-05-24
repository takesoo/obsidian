---
tags:
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

  

