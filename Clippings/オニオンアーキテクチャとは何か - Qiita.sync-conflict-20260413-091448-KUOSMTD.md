---
category: "[[Clippings]]"
author: "[[by nanamen]]"
title: "オニオンアーキテクチャとは何か - Qiita"
source: https://qiita.com/cocoa-maemae/items/e3f2eabbe0877c2af8d0
clipped: 2023-10-04
published: 2022-03-30
topics: 
tags: [clippings 設計 ドメイン駆動設計 アーキテクチャ クリーンアーキテクチャ オニオンアーキテクチャ]
---

オニオンアーキテクチャはJeffrey Palermo氏により考案されたアーキテクチャパターンである。伝統的な[階層化アーキテクチャ](https://ja.wikipedia.org/wiki/%E5%A4%9A%E5%B1%A4%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3)と[オブジェクト指向](https://ja.wikipedia.org/wiki/%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91)の考え方を踏襲しつつ、これまでよりも保守性、テスト容易性、依存性の点で優れたアプリケーションを構築することを目的としている。本記事ではこのオニオンアーキテクチャとは何かについて[Palermo氏の記事](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)を参考にして考察する。

オニオンアーキテクチャを理解する上で前提となるいくつかの知識を挙げる。

### [](#%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91)オブジェクト指向

オニオンアーキテクチャは[オブジェクト指向](https://ja.wikipedia.org/wiki/%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91)を前提としたアーキテクチャである。

### [](#%E9%9A%8E%E5%B1%A4%E5%8C%96%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3)階層化アーキテクチャ

オニオンアーキテクチャは[多層アーキテクチャ](https://ja.wikipedia.org/wiki/%E5%A4%9A%E5%B1%A4%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3)を利用している。

### [](#%E4%BE%9D%E5%AD%98%E6%80%A7%E9%80%86%E8%BB%A2%E3%81%AE%E5%8E%9F%E5%89%87)依存性逆転の原則

オニオンアーキテクチャでは[依存性逆転の原則](https://ja.wikipedia.org/wiki/%E4%BE%9D%E5%AD%98%E6%80%A7%E9%80%86%E8%BB%A2%E3%81%AE%E5%8E%9F%E5%89%87)がオブジェクトのモデリングに利用されている。

### [](#%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E9%A7%86%E5%8B%95%E8%A8%AD%E8%A8%88)ドメイン駆動設計

Entity, Repositoryといった[ドメイン駆動設計](https://ja.wikipedia.org/wiki/%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E9%A7%86%E5%8B%95%E8%A8%AD%E8%A8%88)で提唱されているモデリングの考え方はオニオンアーキテクチャでも一部利用されている。

Jeffrey Palermo氏はオニオンアーキテクチャを説明する前に、伝統的な階層化アーキテクチャが抱える問題について言及している。  
伝統的な階層化アーキテクチャの一例として、プレゼンテーション層(もしくはui層)、モデル層(もしくはビジネスロジック層)、データアクセス層、infrastructure層からなる階層化アーキテクチャを考えてみる。

[![traditional_tier.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2Fbe6cfd75-d192-9e36-83b8-1c7223d6f12d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3246a3ca1543820179e9b0b4be2508c2)](https://camo.qiitausercontent.com/1ba409b18eb7ede4036f73e6f1e4ad556d8b1078/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f62653663666437352d643139322d396533362d383362382d3163373232336436663132642e706e67)

このアーキテクチャを利用すると、各層にあるロジックは通常その下の層に対して依存していく。加えて、基本的にはプレゼンテーション層、モデル層、データ層からinfrastructure層(File Access, DB Access, ORM, etc...)に対する依存が発生するため、全ての層がinfrastructure層に対して密結合になる可能性がある。infrastructure層にあるロジックに修正が発生した場合は、そのロジックを参照している全ての層のロジックも修正する必要がある。

オニオンアーキテクチャでは階層構造を円で表現する。オニオン(=玉ねぎ)という名称は、おそらくこの円構造からきているものと思われる。  
全ての依存関係は円の中心の層に対して向かう。一方で、中心の層から外側の層へは依存しない。アプリケーションのコア(核)になる層の数は変化しても良いが、ドメインモデル層は常に中心に配置する。

[![onion_tier.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2F0ba5f566-f5bc-cddf-92e6-3bbef5040e42.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ce25b4d3462c6968050253013b21600f)](https://camo.qiitausercontent.com/bf896bd32fe421258c98344b500fee0396125a83/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f30626135663536362d663562632d636464662d393265362d3362626566353034306534322e706e67)

Jeffrey Palermo氏の[説明](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)では階層構造が円で表現されているが、下記のように表現しても問題ないだろう。

[![onion_architecure_custom_tire.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2Fed946eb6-6546-3dc0-2a2e-ab1f53754520.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=98d3986272592c253d27937fd0efda29)](https://camo.qiitausercontent.com/883e1b49f8e686c4461d5cd91237aaccebff986c/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f65643934366562362d363534362d336463302d326132652d6162316635333735343532302e706e67)

## [](#%E5%90%84%E5%B1%A4%E3%81%AE%E5%BD%B9%E5%89%B2)各層の役割

オニオンアーキテクチャの各層の役割について説明する。

### [](#%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E3%83%A2%E3%83%87%E3%83%AB%E5%B1%A4)ドメインモデル層

ビジネスロジックに関連した状態と振る舞いの一体化したオブジェクトを配置する。ドメインモデル層のロジックは、他の層への依存関係を持たない。  
E.g.  
Entity, ValueObjectなど

### [](#%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E5%B1%A4)ドメインサービス層

ビジネスロジックに関わる振る舞いのロジック、interfaceなどを配置する。  
E.g.  
DomainService, IRepositoryなど

### [](#%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E5%B1%A4)アプリケーションサービス層

アプリケーション固有のロジック、一般的によく利用される制御用のinterafaceなどを配置する。  
E.g.  
ApplicationService, IUserSessionなど

### [](#ui-infrastructure%E5%B1%A4)UI, Infrastructure層

伝統的な階層化アーキテクチャで表現されるinfrastructure層のオブジェクト(File Access, DB Access, ORM, etc...)、ドメインサービス層で用意したIRepositoryの実態, MVC, やユニットテストなどをここに配置する。

上述の各層の役割を前提とし、簡易なクラス設計の例を下記に示す。  
Controller、UserSession、Repository、FileAccessorといったオブジェクトはJeffrey Palermo氏の説明を参考に、EntityとVOといったオブジェクトは[Robert Martin氏の説明](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html/)やEric Evans氏の説明を参考にしている。  
[![onion_architecture_sample_class_diagram.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2F78c97b37-a4b7-f432-6e1f-9b616a5d3a8f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6fa95466d510c127a573f30983cc3b3d)](https://camo.qiitausercontent.com/4458dadd3ecd04c31495f1d029e6ab204cd0ec20/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f37386339376233372d613462372d663433322d366531662d3962363136613564336138662e706e67)

[Jeffrey Palermo](https://jeffreypalermo.com/author/jeffreypalermo/)氏によると、下記がオニオンアーキテクチャの教義として挙げられている。

-   アプリケーションのロジックは独立したドメインモデルを取り囲むように配置される。
-   infrastructure層と別の内側の層にinterfaceを定義する。interfaceを定義した層とは別の外側の層がinterfaceを実装する。
-   (依存性逆転の原則を利用しているため)依存の方向は常に外側の層から内側の層に向かう。
-   application coreのロジック(\*注1)はinfrastructure層から切り離される。

[Jeffrey Palermo](https://jeffreypalermo.com/author/jeffreypalermo/)氏によると、オニオンアーキテクチャと伝統的な階層化アーキテクチャの違いは、階層間の呼び出し方や依存関係にあると説明されている。

## [](#%E9%9A%8E%E5%B1%A4%E9%96%93%E3%81%AE%E4%BE%9D%E5%AD%98%E9%96%A2%E4%BF%82%E3%81%AE%E9%81%95%E3%81%84)階層間の依存関係の違い

伝統的な階層化アーキテクチャでは、基本的に一つ下の階層に依存する。UI層がData Access層に依存することはない。(\*注2)

[![traditional_architecture_comparison.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2Fee94d890-3bc6-8972-919a-7a8df6534177.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8610078b7011c7b17e678761d14a2b7f)](https://camo.qiitausercontent.com/fccbfbe0c8e85d6faff8c53fca849e8562631a6e/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f65653934643839302d336263362d383937322d393139612d3761386466363533343137372e706e67)

一方オニオンアーキテクチャでは、外側の階層から内側の階層に依存する時、一つ下以外の内側の階層に対して依存することが可能である。

[![onion_architecture_comparison.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2F539e4ae3-8ec4-01a6-fdb5-764a2e106ebb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d8f149af37f5725040445a84ee5edc50)](https://camo.qiitausercontent.com/a1ea96cb826ec93e1374d5d72ba64e4bc73bfd56/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f35333965346165332d386563342d303161362d666462352d3736346132653130366562622e706e67)

## [](#%E5%86%86%E5%BD%A2%E3%81%AB%E3%82%88%E3%82%8B%E4%BE%9D%E5%AD%98%E9%96%A2%E4%BF%82%E3%81%AE%E8%A1%A8%E7%8F%BE)円形による依存関係の表現

[https://qiita.com/cocoa-maemae/items/e3f2eabbe0877c2af8d0#伝統的な階層化アーキテクチャが抱える課題](https://qiita.com/cocoa-maemae/items/e3f2eabbe0877c2af8d0#%E4%BC%9D%E7%B5%B1%E7%9A%84%E3%81%AA%E9%9A%8E%E5%B1%A4%E5%8C%96%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3%E3%81%8C%E6%8A%B1%E3%81%88%E3%82%8B%E8%AA%B2%E9%A1%8C)  
で説明したように、伝統的な階層化アーキテクチャでは、ビジネスロジックやデータアクセス層とinfrastrcture層の依存関係が密になってしまう特徴がある。  
一方オニオンアーキテクチャでは、application coreからinfrastructure層のロジックを呼び出す際常にinterfaceを経由するため、infrastructure層の変更がビジネスロジック(内側の層)に影響を与えることがなくなる。よってビジネスロジックとinfrastrcture層の依存関係は伝統的な階層化アーキテクチャよりも疎になる。

[![onion_dependency.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F55214%2Fd52e3185-1c44-068c-8b0d-69d5924d3ca8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a5e36292c265210505ecdbbfea7634e7)](https://camo.qiitausercontent.com/0e113f46975613326c65611b4af9d61fc8812757/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e61702d6e6f727468656173742d312e616d617a6f6e6177732e636f6d2f302f35353231342f64353265333138352d316334342d303638632d386230642d3639643539323464336361382e706e67)

本記事ではオニオンアーキテクチャについて、階層構造、クラス設計、教義といった観点から説明してきた。オニオンアーキテクチャで新規性があるのは、依存性逆転の法則を利用してinfrastructure層とui(presentation)層を同じ階層位置として捉える点(\*注3)と、ビジネスロジックを配置するdomain層とinfrastructure層の依存関係を疎にする点にあると思われる。この考え方は、オニオンアーキテクチャよりも後に提唱され、日本でも大ヒットしたクリーンアーキテクチャにも取り入れられている。

(\*注1)Jeffrey Palermo氏のブログによると、application coreはapplication service, domain service, domain modelの３層に分離されているが、それぞれの層がどんな役割を持っているかまでは明確に説明されていない。

(\*注2)Jeffrey Palermo氏はこの様に主張しているが、従来型の階層化アーキテクチャでもUI層からData Access層(ORMなど)を直接呼ぶことは可能であるため、この主張には疑問が残る。

(\*注3)  
オニオンアーキテクチャよりも前に提唱されたヘキサゴナルアーキテクチャも同じようにinfrastructure層とui(presentation)層を対称的な関係として捉えていた。

-   [https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)
-   [https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/](https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/)
-   [https://www.codeguru.com/csharp/csharp/cs\_misc/designtechniques/understanding-onion-architecture.html](https://www.codeguru.com/csharp/csharp/cs_misc/designtechniques/understanding-onion-architecture.html)
-   [https://tech.ovoenergy.com/onion-architecture/](https://tech.ovoenergy.com/onion-architecture/)
-   [https://medium.com/@sergiis/stem-in-onion-architecture-or-fallacy-of-data-layer-9923f398f215](https://medium.com/@sergiis/stem-in-onion-architecture-or-fallacy-of-data-layer-9923f398f215)
-   [https://blog.tai2.net/hexagonal\_architexture.html](https://blog.tai2.net/hexagonal_architexture.html)
-   [https://qiita.com/gki/items/91386b082c57123f1ba0](https://qiita.com/gki/items/91386b082c57123f1ba0)