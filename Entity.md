---
tags:
  - オニオンアーキテクチャ
---
[[ビジネスルール]]をカプセル化する[[ドメインオブジェクト]]。
識別子を持つ。
プロパティは処理を通して変化させる。
[[リポジトリ（DDD）|Repository]]を通じてデータベースなどの外部リソースで永続化される。
```ts
imoprt { Money } from "./Money";

export class User {
  private monies: Money[]
  public readonly uuid: string

  constructor(args: { monies: Money[], uuid: string }) {
    this.monies = args.monies;
    this.uuid = args.uuid;
  }

  public isEqual(user: User) {
    this.uuid === user.uuid
  }

  public getMonies() {
    return this.monies
  }

  public addMoney(money: Money) {
    const targetMoney = this.monies.find(m => m.currencyType === money.currencyType)
    
    if(targetMoney) {
      this.monies = this.monies.map(m => {
        return m.currencyType === money.currencyType ? new Money({ currencyType: m.currencyType, amount: m.amount + money.amount }) : m
      }) 
    } else {
      this.monies.push(money)
    }
  }
  
  public subtractMoney(money: Money) {
    const targetMoney = this.monies.find(m => m.currencyType === money.currencyType)
    
    if(!targetMoney) throw new Error('指定された通貨タイプのお金が見つかりません');
    this.monies = this.monies.map(m => {
      if(m.currencyType === money.currencyType) {
        if(m.amount < money.amount) throw new Error('所持金が足りません');
        return new Money({ currencyType: m.currencyType, amount: m.amount - money.amount }) 
      } else { 
        return m 
      }
    })
  }
}

```