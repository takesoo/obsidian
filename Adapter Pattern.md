---
tags:
  - デザインパターン
aliases:
  - アダプターパターン
  - Wrapper Pattern
  - Addapter
  - Wrapper
---
「すでに提供されているもの」と、「必要なもの」との間に入って、その間を埋めるのがアダプターの仕事。
Wrapperともいう
## クラスによるAdapterパターン
PrintBannerクラスがAdapter
![[スクリーンショット 2024-10-07 14.52.21.png]]
Bannerクラスを拡張（extends）して、showWithParenメソッドとshowWithAsterメソッドを継承し、また、Printインターフェースを実装(implements)してprintWeekメソッドとprintStrongメソッドを実装する。
こうすることでMainクラスからはPrintBannerの内部実装を隠すことでき、PrintBannerクラスの[[修正性|変更容易性]]を向上させることができる。
![[スクリーンショット 2024-10-07 14.57.25.png]]
## インスタンスによるAdapterパターン