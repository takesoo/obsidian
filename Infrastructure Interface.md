---
tags:
  - オニオンアーキテクチャ
---
インフラレイヤーで実装していくクラスのインターフェース。
[[Application Service]]レイヤーなどではこのインターフェースに依存した実装をしていくことで、インフラレイヤーの詳細を抽象化して隠蔽する。
```ts
import { User } from "./User"

export interface IUserRepository {
  save(user: User): Promise<User>;
  findByUuid(uuid: string): Promise<User | null>;
}
```
