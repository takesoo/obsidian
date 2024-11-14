---
tags:
  - デザインパターン
aliases:
  - ファサードパターン
  - Facade
---
高度なライブラリやフレームワークに属する広範なオブジェクトを自分のコードに組み込む際にそのまま実装すると、自分のコード中のクラスのビジネスロジックと外部のクラスの実装の詳細が密結合になってしまう。
```mermaid
classDiagram
    class Client {
        +invokeSubsystemA()
        +invokeSubsystemB()
        +invokeSubsystemC()
    }

    class SubsystemA {
        +operationA()
    }
    
    class SubsystemB {
        +operationB()
    }
    
    class SubsystemC {
        +operationC()
    }

    Client --> SubsystemA
    Client --> SubsystemB
    Client --> SubsystemC

```

Facadeが仲介に入り、サブシステムの詳細を隠蔽し、単純なインターフェースをクライアントに提供することで、ビジネスロジックとライブラリの詳細を分離することができる。

```mermaid
classDiagram
	class Client {
	　　+invokeFacade()
	}
    class Facade {
        +operation()
    }
    
    class SubsystemA {
        +operationA()
    }
    
    class SubsystemB {
        +operationB()
    }
    
    class SubsystemC {
        +operationC()
    }

	Client --> Facade
    Facade --> SubsystemA
    Facade --> SubsystemB
    Facade --> SubsystemC

```

ファサードとは建築用語で「建物の正面」や「外観」を意味する
