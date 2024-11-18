---
tags:
  - ruby/gems
  - GraphQL
---
- [[graphql-ruby]]の遅延実行を利用してN+1問題の発生を解消する
```ruby
module Types
 class ArticleType < Types::BaseObject
   field :id, ID, null: false
   field :title, String, null: false
   field :comments, [Types::CommentType], null: false

   def comments
     AssociationLoader.for(Article, :comments).load(object)
   end
 end
end
```
- `for`
	- 引数は`AssociationLoader#initialize`に渡されて、Loaderインスタンスが作成される
	- `Executor`の`@loaders\[key]`にLoaderインスタンスを格納
	- `AssociationLoader`のインスタンスを返す
- `load`
	- `AssociationLoader`の`@queue`に引数のobjectが格納される
	- Promiseインスタンスを生成し、sourceに自身（loaderインスタンス）を格納する
	- `@cache[object]`にPromiseインスタンスを追加する