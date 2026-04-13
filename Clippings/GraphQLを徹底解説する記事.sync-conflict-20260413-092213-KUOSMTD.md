---
category: "[[Clippings]]"
author: "[[Shota Nukumizu九州大学文学部卒業/Python/TypeScript Developerフォロー]]"
title: GraphQLを徹底解説する記事
source: https://zenn.dev/nameless_sn/articles/graphql_tutorial
clipped: 2023-09-06
published: 2023-03-03
topics: 
tags:
  - clippings
  - GraphQL
---

今回の記事では、学習や実務でGraphQLを活用する人を対象に、GraphQLの全体像を把握するためのチュートリアル記事になる。本記事の対象読者は以下の通りである。

-   GraphQLの全体像を把握したい人
-   公式ドキュメントの理解で苦しんでいる人

GraphQLはWeb APIを開発するためのクエリ言語である。REST APIの問題を解決するために、Facebook(Meta)によって開発された。Web APIの開発において、REST APIと比較して柔軟かつ効率的なアプローチを提供できる。さらに、GraphQLではクライアントが必要なデータの構造を定義することができ、サーバから定義したものと同じ構造のデータが返される。

詳細は後述するが、GraphQL最大の特徴は必要以上に大きなデータが返されることを防ぐことにある。これによって、GraphQLは必要最低限のリクエストで必要なdataだけを抽出できる。ところが、柔軟さと豊富な表現と引き換えに、GraphQLで開発されたAPIは複雑になる可能性がある。

GraphQLで開発されたAPIは、データの問い合わせ(query)、書き換え(mutation)や購読(subscription)をサポートしている。これは詳細に

## 概観

GitHubが[GraphQLのAPI](https://docs.github.com/en/graphql)を提供しているため、それに従って説明する。以下は「GraphQLという名前を含むリポジトリを検索して、それに当てはまるリポジトリの数を取得する」クエリだ。

```
query RepositorySearch { 
  search(query: "GraphQL", type: REPOSITORY) {
    repositoryCount
  }
}
```

この結果として、以下のようなJSONが出力される。

```
{
  "data": {
    "search": {
      "repositoryCount": 165655
    }
  }
}
```

GraphQLで開発したAPIからデータを取得する大まかな流れは以下の通りである。

1.  GraphQLは、クライアントがクエリ内容を記述したdocumentを送信し、GraphQLサービスがクエリを実行して結果を送信する。documentはGraphQL query languageを用いて記述。
2.  単一のAPIエンドポイントへこのdocumentをPOSTすることで、クエリが実行される。
3.  実行結果(いわゆるレスポンス)がJSON形式で出力される。

## グラフ理論とGraphQL

GraphQLを学習するにあたって、グラフ理論に関する理解を深めておくとAPIの全体像を把握できる。グラフ理論とは、ノード(頂点)とエッジ(線)の集合で構成されるグラフに関する数学の理論だ。これがどのようにGraphQLに関係しているのだろうか。

![](https://storage.googleapis.com/zenn-user-upload/91aebb3a0845-20230228.png)

GraphQLとグラフ理論の関連性をより理解できるようにするために、TwitterのようなSNSを考えてみる。以下の画像は、マリオを基点に彼のTwitterで繋がっている友人関係をリクエストした時の繋がりを表現している。そのように考えると、このリクエストは以下のような木構造になる。指定したマリオが根っこで、そのマリオに基づく友人関係が枝になるような理解だ。

このリクエストで、マリオは彼の友人同士とエッジで接続されることがわかるだろう。実は、**この構造そのものがGraphQLのクエリによく似ている**。

![](https://storage.googleapis.com/zenn-user-upload/eefe6f8e02c2-20230228.png)

```
木構造
- person
      - name
      - location
      - birthday
      - friends
            * friend name
            * friend location
            * friend birthday
```

```

query {
  me {
    name
    location
    birthday
    friends {
      name
      location
      birthday
    }
  }
}
```

## Schema

GraphQLにはSchemaが存在していて、それによって型システムに基づいたAPIの開発が可能である。例えば、`Person`という型を定義する。

```
type Person {
  name: String!
  age: Int!
}
```

上述のデータ型は2つのフィールドを持っている。

-   `name`：名前。文字列なので`String`のデータ型を使う。
-   `age`：年齢。数字なので`Int`のデータ型を扱う。

`!`をデータ型の後ろにつけることで、データ入力が必須のFieldにできる。

また、データ型同士で別のデータ型を接続することができる。例えば、ブログのアプリケーションに必要なデータ型を想定する。`Person`は新しいデータ型である`Post`に関連したデータ型を以下のように関連付けられる。

```
type Post {
  title: String!
  author: Person!
}
```

データ型`Person`は以下のように編集する。

```
type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}
```

このようにして、GraphQLでは`Person`と`Post`のデータ型を1対1で対応させることもできる。`[]`でデータ型を囲むことで、配列のデータ型を作成することもできる。

## Query

REST APIを使用する場合、データは必ず特定のエンドポイントから読み込まれる。それぞれのエンドポイントでは、返す情報の構造が明確に定義されている。REST APIでは、リソースをURLで表現する。

**一方で、GraphQLで採用されているアプローチは根本的に異なっている。**

GraphQL APIは固定されたデータ構造を返す複数のエンドポイントを持つ代わりに、単一のエンドポイントのみを公開する。GraphQL APIは返されるデータの構造が明確ではない代わりに、クライアントが実際に必要なデータだけを抽出できる。

**要は、クライアントは必要なデータの種類を表現するために、より多くの情報をサーバに送信する必要がある**。この情報をQuery(クエリ)と呼ぶ。

### Queryの基本

クライアントがサーバに送信する具体的なクエリの例を考えてみよう。

```
query {
  allPersons {
    name
  }
}
```

上述の`allPersons`はQueryの根底となるFieldになる。このQueryで指定されているFieldは`name`だけである。このQueryは、現在データベースに保存されているすべての人物のリストを返す。以下はそのレスポンスの例だ。

```
{
  "allPersons": [
    { "name": "Johnny" },
    { "name": "Sarah" },
    { "name": "Alice" }
  ]
}
```

それぞれのレスポンスには`name`しか出力されていないが、他のFieldが出力されていない。Queryで指定されたFieldが`name`のみだったからだ。

仮に、クライアントが`age`等の他のFieldを必要としているなら、Queryを少しだけ調整するだけでいい。

```
{
  allPersons {
    name
    age
  }
}
```

これを出力すると、以下のようにJSONが出力される。

```
{
  "allPersons": [
    { "name": "Johnny", "age": 40 },
    { "name": "Sarah", "age": 23 },
    { "name": "Alice", "age": 34 }
  ]
}
```

このように、GraphQLでは**Queryを調整することで必要なデータの種類をJSON形式で出力できる**。

## Mutation

サーバに情報を要求するだけではなく、ほとんどのアプリケーションではバックエンドに保存されているデータを変更する方法が必要だ。GraphQLでは、データの変更はMutationを使用して実施される。

GraphQLにおけるMutationには以下の3種類が存在する。

-   データの新規作成
-   データの上書き
-   データの削除

例えば、以下のGraphQLのコードを考える。

```

type Query {
  allPersons(last: Int): [Person!]!
  allPosts(last: Int): [Post!]!
}


type Mutation {
  createPerson(name: String!, age: Int!): Person!
  updatePerson(id: ID!, name: String!, age: String!): Person!
  deletePerson(id: ID!): Person!
}

type Person {
  id: ID!
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  title: String!
  author: Person!
}
```

データを新規で作成する場合は、予約語`mutation`で以下のように記述する。`createPerson`という関数を作成して、引数に必要なFieldとその値を指定する。**このとき、`id`はデータが新規作成された後は自動で指定されるので表記する必要はない**。

```
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}
```

## Subscription

Subscriptionとは、GraphQLサーバにデータ変化等の特定のイベントが生じるたびにクライアント側に通知を送信する仕組み。主にチャットアプリの通知で活用されている。

予約語`subscription`を使って、以下のように記述する。

```
subscription {
  newPerson {
    name
    age
  }
}
```

このようにSubscriptionを定義しておくと、新規で`Person`のデータが作成された場合に自動でサーバに情報が追加される。

## Argument

ArgumentはGraphQLにおけるクエリ制御である。これを簡単に言えば、**特定のデータの値に合致したデータを出力する仕組み**である。仕組みとしては、データベースのフィルタリングに近い。

以下の例では、都道府県名と天気を使って、Argumentを用いて都道府県と街のリストから`Tokyo`に含まれる`sunny`の街を取得する。

```
query {
  prefecture(name: "Tokyo") {
    prefName
    cities(weather: "sunny") {
      cityName
      rainyPercent
}}}
```

このQueryを実行すると、以下のようにJSONが出力される。

```
{
  "prefecture": {
    {
      "prefName": "Tokyo",
      "cities": [
        {
          "cityName": "Shinjuku",
          "rainyPercent": 10
        },{
          "cityName": "Ikebukuro",
          "rainyPercent": 0
}]}}}
```

## メリット

-   **柔軟性**: GraphQLはクライアントが必要なデータだけを取得できるように設計されており、過剰なデータの取得を防止できる。また、リクエストに必要なデータの形式や量をクライアントが指定できるため、APIの使用がより柔軟になる。
-   **パフォーマンス**: GraphQLはREST APIよりも効率的にデータを取得できるため、パフォーマンスが向上する。これは、GraphQLが単一のエンドポイントから必要なデータをまとめて取得できるためだ。
-   **Schema駆動開発**: GraphQLはSchema駆動開発に適しています。SchemaによりAPIの設計が明確になり、クライアントとサーバーの間のコミュニケーションが改善されます。
-   **ドキュメンテーション**: GraphQLは自己記述型のAPIであるため、簡単にAPIのドキュメントを生成できる。これにより、APIの使用方法がより明確になり、APIの仕様を理解しやすくなる。
-   **クライアント側の自由度**: GraphQLはクライアントが必要なデータだけを取得できるため、クライアント側での処理が簡潔になる。また、GraphQLは複数の言語やプラットフォームで利用できるため、クライアント側の自由度が高まる。
-   **バージョン管理**: GraphQLはバージョン管理が容易であるため、APIのアップグレードや変更を簡単にできる。これは、GraphQLがスキーマ駆動開発に適しているためだからだ。

## デメリット

-   **学習コストの高さ**: GraphQLはREST APIよりも複雑であり、学習コストが高いことが課題点の一つだ。特に、GraphQLのスキーマ定義には時間がかかる。また、GraphQLを適切に設計するには、REST APIよりも深い理解が必要だ。
-   **セキュリティ**: GraphQLはクエリの柔軟性が高いため、不正なクエリが実行される可能性がある。それゆえにセキュリティの問題がある。クエリのバリデーション、認証、アクセス制御を適切に設定しなければならない。
-   **パフォーマンスの問題**: GraphQLの柔軟性は、過剰なデータの取得につながる可能性があるため、パフォーマンスの問題が考慮される。特に、クライアントが大量のデータを要求した場合、サーバーの負荷が高くなる。
-   **キャッシュの問題**: REST APIでは、クライアント側でキャッシュを利用することができる一方で、GraphQLではクエリを自由自在に調整できるため、キャッシュの利用が難しいという課題点が存在する。このため、GraphQLでは、キャッシュを有効にするための戦略を選択する必要がある。
-   **ツールやライブラリの不足**: GraphQLは比較的新しい技術であり、REST APIに比べて、ツールやライブラリの不足が課題点だ。GraphQL APIを開発する際に、必要な機能を自分で実装しなければならない。

[The GitHub GraphQL API|The GitHub Blog](https://github.blog/2016-09-14-the-github-graphql-api/)によると、GraphQLがWeb開発で使われる理由は以下の通りだ。

-   クライアント側の画面を描画しようとすると、複数回HTTPリクエストを送信しなければならないという課題があったから。
-   APIの実装コードからドキュメントを生成したいから。
-   ユーザが送信するAPIリクエストのパラメータを型安全に扱えるようにしたいから。
-   それぞれのAPIエンドポイントに必要とされるOAuthのスコープを集めたいから。

その他にも、GraphQLがWeb開発で使われる理由には主に以下のようなものがある。

-   **パフォーマンス**: GraphQLは、必要なデータだけを取得することができるため、アプリケーションのパフォーマンスが向上する。REST APIでは、複数のエンドポイントから必要なデータを取得するため、APIリクエストの数が多くなり、パフォーマンスが低下することがあるため。
-   **柔軟性**: GraphQLは、クライアント側が必要なデータを指定するため、アプリケーションのフレキシビリティが向上する。REST APIでは、サーバー側で定義されたエンドポイントからしかデータを取得できない。
-   **クライアント側のカスタマイズ**: GraphQLは、クライアント側が必要なデータを指定できるため、カスタマイズされたクライアントアプリケーションを作成できる。これにより、ユーザーが必要な情報を簡単に取得できるようになる。
-   **ドキュメンテーション**: GraphQLは、ドキュメンテーションを自動的に生成できるため、開発者がAPIの使い方を理解するのに役立ちます。また、GraphQLは、GraphQL Playgroundなどのツールを使用して、APIのクエリやスキーマを簡単にテストできる。
-   **リアルタイム通信**: GraphQLは、Subscription機能でリアルタイムなデータ通信を実現できる。これにより、リアルタイムなアプリケーションの開発が容易になる。

## クライアント

-   REST APIでは、複数のエンドポイントから必要なデータを取得するため、アプリケーションのパフォーマンスが低下する。しかし、GraphQLは、**必要なデータだけを取得できるため**、アプリケーションのパフォーマンスが向上します。
-   REST APIでは、サーバー側で定義されたエンドポイントからしかデータを取得できない。しかし、**GraphQLは、クライアント側が必要なデータを指定できる**ため、アプリケーションの柔軟性がする。
-   REST APIでは、クライアントアプリケーションとサーバーAPIの間でコミュニケーションが必要だ。しかし、GraphQLでは、**単一のエンドポイントと単一の仕様**でクライアントとサーバーが通信できる。

## サーバサイド

-   従来のドメインAPIでは、微細なユースケースにも対応する必要があったものの、**GraphQLでは必要なフィールドだけを定義できる**ため、APIの開発やメンテナンスのコストを削減できる。
-   REST APIでは、クライアント側の変更に合わせてAPIを変更する必要があります。しかし、**GraphQLはクライアントが必要なデータを指定できる**ため、API仕様の変更が減り、コミュニケーションコストが削減される。
-   GraphQLでは**Schemaがドキュメントになるため**、APIのドキュメンテーション作業が不要になる。
-   GraphQLでは、**1フィールド毎、1タイプ毎に開発言語や実行環境、データストアを選択できるため**、クラウド、サーバレスやマイクロサービスのアーキテクチャのポテンシャルを引き出しやすくなる。
-   GraphQLでは、**データソースを切り替えることが容易であり**、状況に応じてデータソースを変更できる。

GraphQLからデータを取得する流れを図でまとめると、以下のようになる。

![](https://storage.googleapis.com/zenn-user-upload/a725ad839825-20230228.png)

今回の記事では、GraphQLの基礎知識やメリット・デメリットを網羅的に解説した。しかし、**GraphQLはあくまでAPIを開発する手段の1つに過ぎない**。様々な要素や可能性を考慮して、自分が着手しているプロダクトにGraphQLが相応しいかどうかを判断することがより重要だ。

-   [https://graphql.org/learn/](https://graphql.org/learn/)
-   [https://ja.wikipedia.org/wiki/GraphQL](https://ja.wikipedia.org/wiki/GraphQL)
-   [https://ja.wikipedia.org/wiki/グラフ理論](https://ja.wikipedia.org/wiki/%E3%82%B0%E3%83%A9%E3%83%95%E7%90%86%E8%AB%96)
-   [https://qiita.com/bananaumai/items/3eb77a67102f53e8a1ad](https://qiita.com/bananaumai/items/3eb77a67102f53e8a1ad)
-   [https://qiita.com/saboyutaka/items/1ecbce27b0a224772a5f](https://qiita.com/saboyutaka/items/1ecbce27b0a224772a5f)
-   [https://zenn.dev/yoshii0110/articles/2233e32d276551](https://zenn.dev/yoshii0110/articles/2233e32d276551)
-   [https://reffect.co.jp/html/graphql](https://reffect.co.jp/html/graphql)
-   [https://circleci.com/ja/blog/introduction-to-graphql/](https://circleci.com/ja/blog/introduction-to-graphql/)
-   [https://www.wallarm.com/what/what-is-graphql-definition-with-example](https://www.wallarm.com/what/what-is-graphql-definition-with-example)
-   [https://www.howtographql.com/basics/2-core-concepts/](https://www.howtographql.com/basics/2-core-concepts/)
-   [https://github.blog/2016-09-14-the-github-graphql-api/](https://github.blog/2016-09-14-the-github-graphql-api/)