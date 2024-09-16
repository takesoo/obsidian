---
aliases:
  - リポジトリ
  - Repository
tags:
  - DDD
---
日本語訳は「保存庫」。オブジェクトを保管しておき、必要な時に取り出せるという役割を持ったコンポーネント。データベースへのアクセスを隠蔽する。

```java
/*
UserRepositoryは「Userの集合（コレクション）」を抽象化した概念で、Userを保管したり取り出したりできる。
UserRepositoryの裏側にはDBなどの永続化技術があるが、使う側にはそのことを意識させない。
UserRepositoryに対してUserはエンティティ(Entity)。
*/

// オブジェクトを保存
val user: User = ...
userRepository.save(user)

// 保存したオブジェクトを取得
val user = userRepository.findById(userId)
```
