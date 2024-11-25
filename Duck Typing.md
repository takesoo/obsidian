---
aliases:
  - ダックタイピング
---
- オブジェクトが持つメソッドやプロパティによって振る舞いを決定するプログラミングスタイル
- 動的型付け言語特有のもの。静的型付け言語の場合はインターフェースを利用して実装する。
```python
class Duck:
    def quack(self):
        print("Quack!")

class Dog:
    def quack(self):
        print("Woof in a duck-like way!")

def make_it_quack(animal):
    animal.quack()

make_it_quack(Duck())  # Quack!
make_it_quack(Dog())   # Woof in a duck-like way!

```

