---
tags:
  - GraphQL
  - Ruby_on_Rails
  - ruby/gems
Link:
  - https://graphql-ruby.org/
  - https://zenn.dev/igaiga/books/rails-practice-note/viewer/rails_graphql_workshop
---
## What
GraphQLをruby on railsに導入するgem

## 準備
graphql gemを使用する
`$ bin/rails g graphql:install`するとディレクトリが生成される。一部を解説。

ファイル|内容
-|-
app/controllers/graphql_controller.rb|コントローラー
app/graphql/mutations/|mutation関係のファイルを格納するディレクトリ
app/graphql/types/|型定義ファイルを格納するディレクトリ
app/graphql/types/query_type.rb|クエリを定義するファイル


## fetch
graphql gemではスキーマはrailsのモデルが担当、フィールドはtypeファイル、クエリはQueryTypeファイルに記述する
```ruby
# models/user.rb
class User < ApplicationRecord
	# name  :string   not null
	# email :string
end

# app/graphql/types/user_type.rb
module Types
	class UserType < Types::BaseObject
		field :id, ID, null: false
		field :name, String, null: false
		field :email, String
	end
end

# app/graphql/types/query_type.rb
module Types
	class QueryType < Types::BaseObject
		field :user, Types::UserType, null: false do
			argument :id, ID, required: true
		end

		def user(id:)
			User.find(id)
		end
	end
end
```

このままだとQueryTypeが肥大化していくのでResolverで分割する
```ruby
# app/graphql/resolvers/user_resolver.rb
module Resolvers
	class UserResolver < GraphQL::Schema::Resolver
		description 'Find a User by ID'
		type Types::UserType, null: false

		argument :id, ID, required: true

		def resolve(id:)
			User.find(id)
		end
	end
end

# app/graphql/types/query_type.rb
module Types
	class QueryType < Types::BaseObject
		field :user, resolver: Resolvers::UserResolver
	end
end
```

## association
Userと関連を持つBookモデルを追加する
```ruby
# app/models/book.rb
class Book < ApplicationRecord
	# title  :string   not null
	# user_id :bigint
end

#app/graphql/types/book_type.rb
module Types
	class BookType < Types::BaseObject
		field :id, ID, null: false
		field :title, String, null: false
		field :user_id, Integer, null: false
	end
end

# app/graphql/types/user_type.rb
module Types
	class UserType < Types::BaseObject
		...
		field :books, [BookType], null: false
	end
end

# app/graphql/resolvers/books_resolver.rb
module Resolvers
	class BooksResolver < GraphQL::Schema::Resolver
		description 'Find Books'
		type [Types::BookType], null: false

		def resolve
			Book.all
		end
	end
end

# app/graphql/types/query_type.rb
module Types
	class QueryType < Types::BaseObject
		field :user, resolver: Resolvers::UserResolver
		field :books, resolver: Resolvers::BooksResolver
	end
end

```

このままではN+1が発生するのでGraphQL::Batchで解消する
```
gem "graphql-batch"
```

```ruby
# app/graphql/types/user_type.rb
module Types
	class UserType < Types::BaseObject
		...
		field :books, [BookType], null: false

		def books
			Loaders::AssociationLoad.for(User, :books).load(object)
		end
	end
end

#app/graphql/types/book_type.rb
module Types
	class BookType < Types::BaseObject
		field :user, UserType, null: false

		def user
			Loaders::RecordLoader.for(User).load(object.user_id)
		end
	end
end
```

## mutation
```ruby
# app/graphql/types/mutation_type.rb
module Types
	class MutationType < Types::BaseObject
		field :create_user, mutation: Mutations::CreateUser
	end
end

# app/graphql/mutations/create_user.rb
module Mutations
	class CreateUser < BaseMutation
		field :user, Types::UserType, null: false

		argument :name, String, required: true
		argument :email, String, required: false

		def resolve(**args)
			user = User.create!(args)
			{
				user: user
			}
		end
	end
end
```

## implements
https://graphql-ruby.org/type_definitions/interfaces#implementing-interfaces
直訳すると「実装する」
他のファイルで定義したInterfaceを実装することができる
```ruby
module Types::RetailItem
	include Types::BaseInterface
	field :price, Types::Price
end

class Types::Car < Types::BaseObject
	implements Type::RetailItem
	...
end
```
