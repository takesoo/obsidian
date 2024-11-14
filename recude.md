---
tags:
  - 英単語
---
- 減らす
- 集約する。要約する

[[関数型プログラミング]]における「集約」
```js
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue,
);

console.log(sumWithInitial);
// Expected output: 10
```

`createReducerContext`は、複数の操作やデータを一つに集約して状態を管理する。