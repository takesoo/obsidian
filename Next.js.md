---
tags:
  - nextjs
---
> Next.js は フルスタックの Web アプリケーションを構築するための React フレームワークです。ユーザーインターフェイスの構築には React Components を使用し、追加機能や最適化には Next.js を使用します。
> 
> さらに Next.js は、バンドルやコンパイルなどのツールも抽象化し、自動的に設定します。これにより、ツールの設定に時間をかけることなく、アプリケーションの構築に集中できます。
> 
> Next.js は個人の開発者であれ、大規模なチームの一員であれ、インタラクティブでダイナミック、かつ高速な Web アプリケーションの構築を支援します。
> 
> [初めに | Next.js 公式ドキュメント 日本語翻訳プロジェクト](https://ja.next-community-docs.dev/docs/app-router/getting-started/)

Next.jsは[[React]]の実行に必要な[[Webpack]]や[[Babel]]やらなんやらが全てパッケージされたフレームワーク

1. **[[サーバーサイドレンダリング]] (SSR)**：Next.jsは、初回のページロード時にサーバー側でページをレンダリングする機能を提供します。これにより、クライアント側でのレンダリングと比べてSEOやパフォーマンスが向上します。

2. **[[スタティックサイトジェネレーション]] (SSG)**：ビルド時にページを静的に生成し、サーバーにホストすることで、さらに高速なページロードを実現します。

3. **[[API Routes]]**：Next.jsにはAPIルートがあり、サーバーサイドでAPIエンドポイントを簡単に定義できます。これにより、サーバーレス機能を提供し、バックエンドのロジックを統合できます。

4. **ファイルベースのルーティング**：Next.jsでは、ファイルシステムに基づいたルーティングを提供し、ディレクトリ構造に基づいて自動的にルートを作成します。


## Tutorial
https://nextjs.org/learn/basics/create-nextjs-app


## [[App Router]] vs [[Pages Router]]
> Next.js には、App Router と Pages Router の 2 種類の Router があります。App Router は、Server Components やストリーミングなど、React の最新機能を利用できる新しい Router です。Pages Router は、サーバーレンダリングされた React アプリケーションをビルドできる Next.js オリジナルのルーターで、古い Next.js アプリケーションも引き続きサポートされています。
> 
> [初めに | Next.js 公式ドキュメント 日本語翻訳プロジェクト](https://ja.next-community-docs.dev/docs/app-router/getting-started/#app-router-vs-pages-router)

App RouterかPages Routerかでファイル構成やルールが全然違う

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