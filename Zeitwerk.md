---
Tags:
  - Ruby_on_Rails
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
[

定数の自動読み込みと再読み込み (Zeitwerk) - Railsガイド

定数の自動読み込みや再読み込みの動作について解説します。(Zeitwerk モード)

![](https://railsguides.jp/railsguides/images/favicons/android-icon-192x192.png)https://railsguides.jp/autoloading_and_reloading_constants.html

![](https://railsguides.jp/images/cover_for_facebook.png)](https://railsguides.jp/autoloading_and_reloading_constants.html)

Ruby on Railsではいちいちrequireを書くことはなく、Zeitwerkローダーが管理している

たとえば`users_controller.rb`で定義されたクラスがUsersControllerであれば自動で読み込まれる

config.eager_loading

アプリケーション起動時にコードを全て読み込んでおく設定。レスポンスが早くなる。