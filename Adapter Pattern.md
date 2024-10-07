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
特に「すでに提供されているもの」が十分にテストされていて、バグが少なく、これまで使われてきた実績があるなら、このパターンは有用。
Wrapperともいう
## クラスによるAdapterパターン
Bannerクラスが「すでに提供されているもの」で、Printインターフェースが「必要なもの」。PrintBannerクラスがAdapter。
![[スクリーンショット 2024-10-07 14.52.21.png]]
Bannerクラスを拡張（extends）して、showWithParenメソッドとshowWithAsterメソッドを継承し、また、Printインターフェースを実装(implements)してprintWeekメソッドとprintStrongメソッドを実装する。
こうすることでMainクラスからはPrintBannerの内部実装を隠すことでき、PrintBannerクラスの[[修正性|変更容易性]]を向上させることができる。
![[スクリーンショット 2024-10-07 14.57.25.png]]
## インスタンスによるAdapterパターン
「継承」の代わりに「委譲」をつかう。PrintBannerクラスがprintWeakメソッドとprintStringメソッドの処理をBannerに委譲している。
![[スクリーンショット 2024-10-07 17.11.03.png]]
![[スクリーンショット 2024-10-07 17.12.02.png]]
![[スクリーンショット 2024-10-07 17.12.10.png]]