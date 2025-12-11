---
category: "[[Clippings]]"
author: "[[Home]]"
title: 🔖 Arcブラウザでブックマークレットを使う
source: https://namaraii.com/post/2023/20230815/43750acf903ac6d0758e/
clipped: 2023-09-25
published:
tags:
  - clippings
  - bookmarklet
  - chrome/extension
---

📅 2023年8月15日

ここ最近[Arcブラウザ](https://arc.net/)をメインに使っている。タブが縦に並ぶところも良いし、操作性や性能面などなかなか気に入っている。

ただ、Arcはブックマークレットをサポートしていない。どうしても使いたい自作のブックマークレットがひとつあり、その場合にだけChromeを立ち上げていた。

しかし、どう考えてもこれは違うだろう…ということで良い方法がないか調べて見たところ、以下のページをみつけた。

-   [Get Bookmarklets to work : ArcBrowser](https://www.reddit.com/r/ArcBrowser/comments/118ggqt/get_bookmarklets_to_work/)

このページではブックマークレットをChrome拡張に変換する以下のサービスを紹介している。

-   [Convert bookmarklet to Chrome extension](https://sandbox.self.li/bookmarklet-to-extension/)

実際に使っているブックマークレットで試してみた。

-   名称、説明を入力する
-   ブックマークレットをDrag&Dropして`Generate Extension`を押す
-   機能拡張が生成されzipがダウンロードされるので解凍する
-   Arcブラウザのメニューで`Extensions` - `Manage Extenions`を選ぶ
-   開いた画面で`Developer mode`をオンにする
-   `Load unpacked`ボタンを押し、解凍したzipフォルダを選ぶ

という手順で無事ブックマークレットを機能拡張として使用できた。

[←⌨️ 久しぶりにChrome拡張を書いた](https://namaraii.com/post/2023/20230816/ffc8c4cc789f42e81824/) [🌀 台風７号→](https://namaraii.com/post/2023/20230814/8fa23ea47e08c2109412/)