---
tags:
  - Rails
  - デザインパターン
aliases:
  - クエリオブジェクトパターン
---
- 検索ビジネスロジックをコントローラーから分離するデザインパターン
- スコープが複数のカラムとのやりとりをする場合や、他のテーブルとJOINする場合はQuery Objectへの移行を検討するべき
- 使用上のルール
	- 命名規則をひとつに定めること
		- `WithRecentlyCreatedProjectQuery`
		- [[目的駆動名前設計]]
	- リレーションを返す`.call`メソッドをQueryObjectの呼び出しに使うこと
	- オブジェクトなどのリレーションは常に第一引数で受け取ること
	- 追加オプションを受け取れるようにすること
	- 読みやすいクエリメソッドを書くことに集中すること
	- QueryObjectを名前空間でグループ化すること
		- `app/queries/with_recently_created_project_query.rb`
	- 全てのメソッドを`.call`の結果に以上することも検討すること

```ruby
module Users
	class WithRecentlyCreatedProjectQuery
		DEFAULT_RANGE = 2.days

		def self.call(relation = User.all, time_range: DEFAULT_RANGE) 
			relation
				.joins(:projects)
				.where('projects.created_at > ?', time_range.ago)
				.distinct
		end
	end
end
```

---
[Railsで重要なパターンpart 2: Query Object（翻訳）｜TechRacho by BPS株式会社](https://techracho.bpsinc.jp/hachi8833/2022_03_24/47287)