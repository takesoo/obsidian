---
category: "[[Clippings]]"
author: "[[シュンReact・Rails・AWSの技術を中心にWebエンジニアやってます。フォロー]]"
title: "bullet（Gem）はどのようにN+1問題を検知しているのか？について調査してみた"
source: https://zenn.dev/hososhun/articles/e7e0f72faebb65
clipped: 2023-10-05
published: 
topics: 
tags: [clippings]
---

## はじめに

今回は、Railsを使用しているプロジェクトのパフォーマンスチューニングを行い際によく活用される以下の「bullet」というGemのソースコードを実際に読み、どのようなロジックでN+1問題を検知しているのか？について調査してみようと思います。

## そもそもbulletのGemとは？

お馴染みの **N+1問題** と **counter\_cacheを使用する必要がある状態でありながら、counter\_cacheが使用されていない問題** を検知するためのGemになります。

READMEの一部を引用([https://github.com/flyerhzm/bullet](https://github.com/flyerhzm/bullet))

> The Bullet gem is designed to help you increase your application's performance by reducing the number of queries it makes. It will watch your queries while you develop your application and notify you when you should add eager loading (N+1 queries), when you're using eager loading that isn't necessary and when you should use counter cache.

今回の本題であるN+1問題についてはとても有名である為、本記事に目を通して頂いた方であれば既にご存知か思いますが、 **とcounter\_cache** についてはもしかしたら初耳という方もいらっしゃるかと思いましたので、一応概要だけ紹介させて頂きますと以下になります。

### counter\_cacheとは？

RailsのActiveRecordでRDB関連の設定の１つです。  
counter\_cacheを設定することにより、親子関係のテーブルにおいて、親テーブルが子テーブルの  
件数をキャッシュすることができるようになります。

**counter\_cache**の詳細についてもっと知りたい方は以下の記事などを参考にしてみてください。

## 解説

bulletには、N+1問題とcounter\_cache不使用を検知する機能がありますが、今回はN+1問題の検知の方に絞って実際のコードを読み解き、その中でも特に重要だと思われる箇所に絞って解説をさせて頂きたいと思います。  
実際にソースコードを確認してみると、`lib/bullet/detector/n_plus_one_query.rb`というファイルがN+1問題を検知するロジックと深く関係していることが分かりました。

備考  
※今回はあくまで個人の趣味で調査した内容となっている為、あくまで全体のイメージを掴む感覚として捉えて頂けましたら幸いです。

では実際にN+1を検知の役割を担っているコード部分のご紹介です。  
lib/bullet/detector/n\_plus\_one\_query.rb

```


module Bullet
  module Detector
    class NPlusOneQuery < Association
      extend Dependency
      extend StackTraceFilter

      class << self
        
        
        
        
        def call_association(object, associations)
	　　　　
          return unless Bullet.start?
	  
          return unless Bullet.n_plus_one_query_enable?
	  
          return unless object.bullet_primary_key_value
	  
          return if inversed_objects.include?(object.bullet_key, associations)
          
          add_call_object_associations(object, associations)

          
          Bullet.debug(
            'Detector::NPlusOneQuery#call_association',
	    
            "object: #{object.bullet_key}, associations: #{associations}"
          )
	  
          if !excluded_stacktrace_path? && conditions_met?(object, associations)
            Bullet.debug('detect n + 1 query', "object: #{object.bullet_key}, associations: #{associations}")
            create_notification caller_in_project(object.bullet_key), object.class.to_s, associations
          end
        end
	
	
        def conditions_met?(object, associations)
	  
	  
          possible?(object) && !impossible?(object) && !association?(object, associations)
        end

        def possible?(object)
	　　　　
          possible_objects.include? object.bullet_key
        end

        def impossible?(object)
	  
          impossible_objects.include? object.bullet_key
        endi
	
        
	
        def association?(object, associations)
	  
	  
          value = object_associations[object.bullet_key]
          value&.each do |v|
            
            
            
            result = v.is_a?(Hash) ? v.key?(associations) : associations == v
            return true if result
          end

          false
        end

	private
        ・・・
	
        def create_notification(callers, klazz, associations)
          notify_associations = Array.wrap(associations) - Bullet.get_safelist_associations(:n_plus_one_query, klazz)
          if notify_associations.present?
            notice = Bullet::Notification::NPlusOneQuery.new(callers, klazz, notify_associations)
            Bullet.notification_collector.add(notice)
          end
        end
      end
    end
  end
end
```

色々と書いてしまい、ややこしくなってしまいましたが、bulletでN＋1問題を検知しているロジックとしては、以下になるのではと思いました。

### 調査結果

**「N+1問題を検知する設定が行われている状態で、ActiveRecord::Associations::Preloader配下のClassを使用し、関連先のobjectの情報がキャッシュされているかどうかを確認している。」**

## おわりに

この先を把握するには、Railsのリポジトリから`ActiveRecord::Associations::Preloader`などの中身を解読する必要がありそうですが、その辺りはまたの機会とさせて頂きたいと思います。  
本記事を最後まで拝読頂きましてありがとうございました。