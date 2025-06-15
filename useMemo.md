---
tags:
  - react/hooks
---
## what
- 関数の計算結果をキャッシュ（[[メモ化]]）するためのフック
```js
function ExpensiveComponent({ numbers }) {
  // ❌ 毎回計算される
  const sum = numbers.reduce((a, b) => a + b, 0);
  
  // ✅ numbersが変わった時だけ計算
  const memoizedSum = useMemo(() => {
    console.log('計算実行中...');
    return numbers.reduce((a, b) => a + b, 0);
  }, [numbers]);
  
  return <div>合計: {memoizedSum}</div>;
}
```

## why
- 重い計算結果をキャッシュすることでパフォーマンス向上につながる