---
tags:
  - React
aliases:
  - HOC
  - 高階コンポーネント
---
## what
- コンポーネントを引数で受け取って新規のコンポーネントを返すような関数
```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
## why
- 似たようなコンポーネントの共通ロジック部分を共通化するため