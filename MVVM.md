---
tags:
  - アーキテクチャ
---
ソフトウェアの階層モデル
- Model
	扱うデータ
- View
	視覚的な要素
- VewModel
	Modelが持っている情報をViewに表示される値に変更する

![[Pasted image 20240418163739.png|1000]]

動作順
1. ユーザーのActionがViewを通じて入ってくる
2. ViewにActionが入ると、CommandパターンでViewModelに伝える
3. ViewModelはModelにデータを要請する
4. ModelはViewModelから要請されたデータを応答して加工して保存する
5. ViewはViewModelとDataBindingして画面に表示する
![[Pasted image 20240418163722.png|1000]]

メリット
- ViewとModelの依存性を完全に分離できるため、独立性を維持することができます。
- 独立性を維持するため、効率的なユ単体テストができます。
- ViewとViewModelをバインディングするため、コードの量が大幅に減ります。
- ViewとViewModelの関係は1:1です。

デメリット
- 簡単なUIを作る時に設計するのが難しいです。
- データバインディングが必須です。
- 複雑であればあるほど、Controllerと同様にViewModelが複雑になります。