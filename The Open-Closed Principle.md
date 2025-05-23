---
aliases:
  - OCP
---
- クラスなどが、拡張(extension)については開かれている(open)が、修正(modification)については閉じられている(closed)べき
	- クラスを設計するときは、将来の拡張がしやすいことが望まれる。しかし、テストまで十分に実装されているクラスを変更すると品質を下げるリスクがある。そのため、既存のクラスを変更することなく機能拡張できるように設計するべき。
- OCPに乗っ取ったモジュールは変更に強く、再利用性が高くなるため
- インターフェースを用いて設計することで実現できる。呼び出す側のクラスがインターフェースに依存するようにすることで、新しい機能を実装したクラスがインターフェースに準拠していれば、再利用が可能になる
- [[オブジェクト指向]]の[[ポリモーフィズム]]はOCPの実装方法の代表例
- [[ストラテジパターン|Strategy Pattern]]、[[Observer Pattern]]、[[Template Pattern]]、[[Decoratorパターン]]などもOCPの実現例
```mermaid
classDiagram
    class Shape {
        <<interface>>
        +area() float
    }

    class Circle {
        -radius: float
        +area() float
    }

    class Rectangle {
        -width: float
        -height: float
        +area() float
    }

    class AreaCalculator {
        +totalArea(shapes: List~Shape~) float
    }

    Shape <|.. Circle
    Shape <|.. Rectangle
    AreaCalculator --> Shape : uses

```