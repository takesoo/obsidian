---
tags:
  - オニオンアーキテクチャ
---
特定の[[Entity]]に属さない[[ドメインロジック]]をカプセル化させる役割を持つ。
複数のEntityや[[バリューオブジェクト|Value Object]]間で行われる操作や[[ビジネスロジック]]で、単一のEntityに閉じ込めるには適さない処理を担当する。
```ts
import { Money } from "./Money";
import { User } from "./User";

export class UserService {
  public sendMoney(args: { sendUser: User; receiveUser: User; money: Money }) {
    args.sendUser.subtractMoney(args.money)
    args.receiveUser.addMoney(args.money)
  }
}


```