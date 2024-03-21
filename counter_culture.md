---
tags:
  - ruby/gems
  - Ruby_on_Rails
---
[magnusvk/counter_culture: Turbo-charged counter caches for your Rails app.](https://github.com/magnusvk/counter_culture)

- railsの[[カウンターキャッシュ]]機能をより使いやすく拡張したgem
- 既存データをcountして指定のカラムに保存するメソッド`counter_culture_fix_counts`などがある


```ruby
class Product < ActiveRecord::Base
  belongs_to :category
  counter_culture :category

	# == Schema Information
	#
	# Table name: categories
	#
	# product_count :int
end

class Category < ActiveRecord::Base
  has_many :products
end
```