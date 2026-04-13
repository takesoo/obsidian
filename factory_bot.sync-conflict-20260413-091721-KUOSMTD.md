---
aliases: 
tags:
  - ruby/gems
  - factory_bot
cssclasses: 
publish: false
---
https://github.com/thoughtbot/factory_bot

もともとfactory_girlだったのがfactory_botになった→[[【翻訳】"Factory Girl"が"Factory Bot"に変わった理由（と移行手順） - Qiita]]

詳しい使い方は→ https://thoughtbot.github.io/factory_bot

関連gem
- [[factory_trace]]

`after(:build) { warn 'Warning: This factory is deprecated' }`

## ベストプラクティス
- 必須項目だけをfactoryに定義する。それ以外はテストで定義する。
- buildで済むテストはbuildを使う
- created_atやidが必要ならbuild_stabbedを使う
- trait
	- 使いまわせる汎用的な振る舞いのみをtraitに定義する
```
	factory :week_long_published_story, traits: [:published, :week_long_publishing]
```
- ユニークな値はsequenceで生成する
- 現実に値はfakerで生成する
- association先はcreateせず、
```ruby
factory :post do
	user { create(:user) } # 🙅 build(:post)でもクリエイトされる
	user { association user } # 🙆 
end
```