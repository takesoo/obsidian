---
tags:
  - Ruby_on_Rails
---
- ビジネスロジックの実装場所
- コントローラーから呼び出す
- 名前はXXXerが一般的
- callで実行する
- ロジックが詰め込まれがちなので単一責務になるように注意する

```ruby
class SomethingExecuter do
	def call
		execute
	end
end
```