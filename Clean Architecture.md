---
tags:
  - book
Status:
  - 読みかけ
Auther: 
Purpuse: 
Summary: 
Start: 2024-11-09
End:
---

## 第1部 イントロダクション
### 第1章 設計とアーキテクチャ
> ソフトウェアアーキテクチャの目的は、求められるシステムを構築・保守するために必要な人材を最小限に抑えることである。
- コードの増加量とエンジニアの増加量が反比例しているならそのアーキテクチャは不健全である。
> 速く進む唯一の方法は、うまく進むことである。
- 正しく進むこととも言い換えれる。ダーティなコードを量産するのではなく、ソフトウェアアーキテクチャの品質と真剣に向き合い正しく実装することで、アーキテクチャの目的は達成される。
- そのためには優れたアーキテクチャとは何かを知る必要がある。それが本書の目的。
### 第2章 2つの価値のお話
- ソフトウェアシステムがステークホルダーに提供する二つの価値
	- 振る舞い・機能
	- 構造・アーキテクチャ
- 重要なのはアーキテクチャ
	- 今は動作するが変更できない（アーキテクチャが複雑で変更コストが大きいなどの理由で）プログラムは価値を生まないが、動作しなくても変更可能なプログラムは、修正を重ねれば動くようになり価値を生むため。
> ソフトウェア開発チームには、機能の緊急性よりもアーキテクチャの重要性を強く主張する責任が求められる。
- ビジネスマネージャーは機能の緊急性を言及してくるが、ソフトウェア開発者はソフトウェアの保護に対する責任がある。
## 第2部 構成要素から始めよ：プログラミングパラダイム
### 第3章  パラダイムの概要
- アーキテクチャの境界を超えるための仕組みとして[[ポリモーフィズム]]（[[オブジェクト指向]]）、データの配置やアクセスに規制を課すために[[関数型プログラミング]]、モジュールのアルゴリズムの基盤として[[構造化プログラミング]]を使う。
### 第4章 構造化プログラミング
- `goto`文の使用を制限する
- `if/then/else`や`while/until`で順次、選択、反復を基本構成としてプログラミングできるようになった
- これによって機能分割をする
### 第5章 オブジェクト指向プログラミング
- ソフトウェアアーキテクトにおいては、OOとは「ポリモーフィズムを使用することで、システムにあるすべてのソースコードの依存関係を絶対的に制御する能力」である
### 第6章 関数型プログラミング
## 第3部 設計の原則
### 第7章 SRP：単一責務の原則
### 第8章 OCP：オープン・クローズドの原則
### 第9章 LSP：リスコフの置換原則
### 第10章 ISP：インターフェース分離の原則
### 第11章 DIP：依存関係逆転の原則
## 第4部 コンポーネントの原則
### 第12章 コンポーネント
### 第13章コンポーネントの凝集性
### 第14章 コンポーネントの結合
## 第5部 アーキテクチャ
### 第15章 アーキテクチャとは？
### 第16章 独立性
### 第17章 バウンダリー：境界線を引く
### 第18章 境界の解剖学
### 第19章 方針とレベル
### 第20章 ビジネスルール
### 第21章 叫ぶアーキテクチャ
### 第22章 クリーンアーキテクチャ
### 第23章 プレゼンターとHumble Object
### 第24章 部分的な境界
### 第25章 レイヤーと境界
### 第26章 メインコンポーネント
### 第27章 サービス：あらゆる存在
### 第28章 テスト境界
### 第29章 クリーン組み込みアーキテクチャ
## 第6部 詳細
### 第30章 データベースは詳細
### 第31章 ウェブは詳細
### 第32章 フレームワークは詳細
### 第33章 事例：動画販売サイト
### 第34章 書き残したこと
## ロジックツリー
### A
- [[a]]
### B
- b1
- b2
### C
