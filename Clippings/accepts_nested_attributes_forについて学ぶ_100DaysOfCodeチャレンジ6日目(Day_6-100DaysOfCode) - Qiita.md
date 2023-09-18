[https://qiita.com/yuta-ushijima/items/22b823bfa3e2dd358fa0](https://qiita.com/yuta-ushijima/items/22b823bfa3e2dd358fa0)

  

この記事はTwitterで人気のハッシュタグ`\#100DaysOfCode`をつけて、  
100日間プログラミング学習を続けるチャレンジに挑戦した6日目の記録です。

- ruby 2.4.1
- Rails 5.0.1

## 現在学習している内容のリポジトリ

## 本日学んだこと

- accepts_nested_attributes_forの使い方

## accepts_nested_attributes_forとは？

Railsが標準で提供している、ActiveRecordのメソッドの一つ。  
モデル同士が関連付けられている時に、ネストさせることで一度にまとめてレコードの更新ができるようになります。

今回は次のような関連づけられたモデルがあったと仮定します。

Contactモデルを基準として、

- Phoneモデルとは`has_many`
- Kindモデルとは`belongs_to`

の関係にある状態です。

```
# app/models/contact.rbclass Contact < ApplicationRecord  # モデル同士の関連付け  belongs_to :kind  has_many :phones  accepts_nested_attributes_for :phonesend
```

例えばパラメータとして次のような値が渡されたとしましょう。

```
 params = { contact: {       name: 'jack',       email: 'hak@hal.com',       birthdate: '2018/11/02',       kind_id: 2,       phones_attributes: [         {number: '1234' },         {number: '5678'},         {number: '9012'},       ] }}# ハッシュによる戻り値{:contact=>  {:name=>"jack",   :email=>"hak@hal.com",   :birthdate=>"2018/11/02",   :kind_id=>2,   :phones_attributes=>[{:number=>"1234"}, {:number=>"5678"}, {:number=>"9012"}]}}
```

Contactモデルに`accepts_nested_attributes_for :phones`と定義されているので、createメソッドでContactモデルがパラメータを引数としてレコードを作成すると、ネストされているphoneモデルも同時に一緒に作成することができることになります。

```
pry(main)> Contact.create(params[:contact])# 発行されるSQL(0.1ms)  begin transaction  Kind Load (1.4ms)  SELECT  "kinds".* FROM "kinds" WHERE "kinds"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]  SQL (3.8ms)  INSERT INTO "contacts" ("name", "email", "birthdate", "created_at", "updated_at", "kind_id") VALUES (?, ?, ?, ?, ?, ?)  [["name", "jack"], ["email", "hak@hal.com"], ["birthdate", "2018-11-02"], ["created_at", "2018-06-21 13:48:05.178496"], ["updated_at", "2018-06-21 13:48:05.178496"], ["kind_id", 2]]  SQL (2.6ms)  INSERT INTO "phones" ("number", "contact_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["number", "1234"], ["contact_id", 104], ["created_at", "2018-06-21 13:48:05.186941"], ["updated_at", "2018-06-21 13:48:05.186941"]]  SQL (0.1ms)  INSERT INTO "phones" ("number", "contact_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["number", "5678"], ["contact_id", 104], ["created_at", "2018-06-21 13:48:05.197365"], ["updated_at", "2018-06-21 13:48:05.197365"]]  SQL (0.1ms)  INSERT INTO "phones" ("number", "contact_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["number", "9012"], ["contact_id", 104], ["created_at", "2018-06-21 13:48:05.200102"], ["updated_at", "2018-06-21 13:48:05.200102"]]   (5.9ms)  commit transaction# 作成されるContactオブジェクト=> #<Contact:0x007fa63fec7678 id: 104, name: "jack", email: "hak@hal.com", birthdate: Fri, 02 Nov 2018, created_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00, updated_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00, kind_id: 2>
```

最後に作成されたContactオブジェクトをlastメソッドで取得してみましょう。

```
 pry(main)> Contact.last# 発行されるSQLContact Load (0.3ms)  SELECT  "contacts".* FROM "contacts" ORDER BY "contacts"."id" DESC LIMIT ?  [["LIMIT", 1]]# 取得したContactオブジェクト情報=> #<Contact:0x007fa642962270 id: 104, name: "jack", email: "hak@hal.com", birthdate: Fri, 02 Nov 2018, created_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00, updated_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00, kind_id: 2>
```

PhoneモデルはContactオブジェクトに関連付けられているので、`Contact.last.phones`とすればPhoneモデルの情報が取得できます。

```
pry(main)> Contact.last.phones# 発行されたSQLContact Load (0.7ms)  SELECT  "contacts".* FROM "contacts" ORDER BY "contacts"."id" DESC LIMIT ?  [["LIMIT", 1]]  Phone Load (0.5ms)  SELECT "phones".* FROM "phones" WHERE "phones"."contact_id" = ?  [["contact_id", 104]]# id:104をもつContactオブジェクトに紐づいたPhoneオブジェクトの情報を取得=> [#<Phone:0x007fa642b0fca8  id: 412,  number: "1234",  contact_id: 104,  created_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00,  updated_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00>, #<Phone:0x007fa642b0fb40  id: 413,  number: "5678",  contact_id: 104,  created_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00,  updated_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00>, #<Phone:0x007fa642b0f938  id: 414,  number: "9012",  contact_id: 104,  created_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00,  updated_at: Thu, 21 Jun 2018 13:48:05 UTC +00:00>]
```

パラメータでPhoneモデルの情報が次のようにネストされていたのを思い出してください。

ここで渡されたPhoneモデルのnumber属性がContactモデルのレコード作成時に一緒に保存されたというところがポイントです。

ただ、Railsの生みの親であるDHHがこのメソッド自体をdeplicateしようと考えているらしいので、いずれなくなるのかもしれませんね。

> I'd actually like to kill accepts_nested_attributes_for in due time. Don't think we should promote it for this new API. Rather, let's just show how to do it by hand in the controller. [https://github.com/rails/rails/pull/26976#discussion_r87855694](https://github.com/rails/rails/pull/26976#discussion_r87855694)

DHH的には、複雑なことをモデルでやらせるよりも、controller側でシンプルに書くべきだと考えているようですね。

[Active Record Nested Attributes](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html)

[Railsでaccepts_nested_attributes_forとfields_forを使ってhas_many関連の子レコードを作成/更新するフォームを作成](http://ruby-rails.hatenadiary.com/entry/20141208/1418018874)

Why not register and get more from Qiita?

1. We will deliver articles that match youBy following users and tags, you can catch up information on technical fields that you are interested in as a whole
2. you can read useful information later efficientlyBy "stocking" the articles you like, you can search right away

[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fyuta-ushijima%2Fitems%2F22b823bfa3e2dd358fa0&realm=qiita)

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fyuta-ushijima%2Fitems%2F22b823bfa3e2dd358fa0&realm=qiita)

Comments