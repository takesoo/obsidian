---
category: "[[Clippings]]"
author: "[[Qiita]]"
title: "【翻訳】"Factory Girl"が"Factory Bot"に変わった理由（と移行手順） - Qiita"
source: https://qiita.com/jnchito/items/c71b8f66f61214227555
clipped: 2023-08-31
published: 2017-12-11
topics: 
tags: [clippings Rails FactoryGirl FactoryBot]
---

2017年10月に、Rubyのライブラリ(gem)である"Factory Girl"が"Factory Bot"に変わりました。

名前が変わった理由が以下のページに記載されていたので、翻訳してみます。

## [](#%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E5%90%8D%E3%81%AE%E6%AD%B4%E5%8F%B2)プロジェクト名の歴史

### [](#factory-girl)Factory Girl

このライブラリはもともと2008年に"Factory Girl"という名前でリリースされました。

この名前を選んだ理由は書籍「デザインパターン」に載っていたファクトリメソッドパターンとオブジェクトの母（Object Mother）パターンに従っていたことと、ローリング・ストーンズに同じ名前の曲があったためです。

### [](#factory-bot)Factory Bot

"Factory Girl"という名前はこのライブラリの存在を知った一部の開発者を混乱させました。また、不快で問題があるように感じる人々もいました。そのため、2017年10月に私たちはこのライブラリの名前を"Factory Bot"に変更しました。

（翻訳ここまで）

名前が変更される詳しい経緯については以下のissueも参考になります。

-   [Repository Name · Issue #921 · thoughtbot/factory\_bot](https://github.com/thoughtbot/factory_bot/issues/921)

## [](#%E5%8F%82%E8%80%83factory-girl%E3%81%8B%E3%82%89factory-bot%E3%81%B8%E7%A7%BB%E8%A1%8C%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95)参考：Factory GirlからFactory Botへ移行する方法

Factory BotとFactory Girlは名前が違うだけで基本的に同じものです。  
Railsプロジェクトの場合、移行したい場合は次のようにします。

### [](#1-gemfile%E3%82%92%E6%9B%B4%E6%96%B0%E3%81%97%E3%81%A6bundle-install)1\. Gemfileを更新してbundle install

Gemfile

```
- gem 'factory_girl_rails'
+ gem 'factory_bot_rails'
```

### [](#2-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E5%86%85%E3%81%AEfactorygirl%E3%82%92factorybot%E3%81%AB%E7%BD%AE%E6%8F%9B)2\. プロジェクト内のFactoryGirlをFactoryBotに置換

spec/factories/users.rb

```
- FactoryGirl.define do
+ FactoryBot.define do
```

[UPGRADE\_FROM\_FACTORY\_GIRL.md](https://github.com/thoughtbot/factory_bot/blob/4-9-0-stable/UPGRADE_FROM_FACTORY_GIRL.md) にある通り、Railsのプロジェクトルートディレクトリ内で、下記コマンドを打つのが楽です。

```
grep -e FactoryGirl **/*.rake **/*.rb -s -l | xargs sed -i "" "s|FactoryGirl|FactoryBot|"
```

### [](#3-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E5%86%85%E3%81%AEfactory_girl%E3%82%92factory_bot%E3%81%AB%E7%BD%AE%E6%8F%9B)3\. プロジェクト内のfactory\_girlをfactory\_botに置換

```
- g.fixture_replacement :factory_girl, dir: "spec/factories"
+ g.fixture_replacement :factory_bot, dir: "spec/factories"
```

こちらも下記コマンドが楽です。

```
grep -e factory_girl **/*.rake **/*.rb -s -l | xargs sed -i "" "s|factory_girl|factory_bot|"
```

### [](#4-%E3%83%86%E3%82%B9%E3%83%88%E3%81%8C%E3%81%99%E3%81%B9%E3%81%A6%E3%83%91%E3%82%B9%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%82%92%E7%A2%BA%E8%AA%8D)4\. テストがすべてパスすることを確認