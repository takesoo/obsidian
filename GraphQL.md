---
tags:
  - GraphQL
関連ページ: "[[GraphQLを徹底解説する記事]]"
---
## GraphQLとは
> # A query language for your API
>GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools
>https://graphql.org/

GraphQLはAPIのための[[クエリ言語]]であり、既存データを使ってクエリを実行するための[[ランタイム]]である。GraphQLはあなたのAPIのデータについて完璧で分かりやすい記述を提供し、必要なものだけを要求する力をクライアントサイドに与え、時間の経過とともにAPIの開発をより簡単にし、デベロッパーツールをパワフルに後押しします。

### GraphQLはクエリ言語
クライアントサイドで自由にクエリを作ってAPIにリクエストすることで、必要なプロパティを過不足なく要求して得ることができる。
従来の[[RestAPI]]ではサーバーサイドで定義したプロパティのみを返す。そのため、プロパティが増えたり減ったりするたびにサーバーサイドがコードを変更する必要があった。
GraphQLを用いるとレスポンスするプロパティはクエリで決める。そのためプロパティの増減があってもサーバーサイドがコードを変更する必要がなく、開発のスピードは向上する。
## 構造
>A GraphQL service is created by defining types and fields on those types, then providing functions for each field on each type
>https://graphql.org/learn/

GraphQLサービスは、型とその型のフィールドを定義し、各型の各フィールドに関数を提供することで作成される。

### Schema
スキーマがあり、スキーマはフィールドを持っている。それぞれ型を定義できる。
```
type Person {
	name: String
	age: Int
	posts: [Post]
}

type Post {
	title: String
	author: Person
}

type Query {
	people(last: Int): [Person]
	allPosts(last: Int): [Post]
}

type Mutation {
	createPerson(name: String, age: Int): Person
}
```

リクエストを解析してスキーマの型定義からレスポンスを柔軟に作成する

## Ruby on RailsにGraphQLを導入する
→[[GraphQL Ruby]]

