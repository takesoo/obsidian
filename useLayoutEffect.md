---
tags:
  - react/hooks
---
## what
- 
- [[useEffect]]の一種
- パフォーマンスを低下させる可能性があるので、[[useEffect]]の使用を推奨
```ts
function Component() {
  // useEffect: 画面更新後に実行（非同期）
  useEffect(() => {
    console.log('画面更新後');
  });
  
  // useLayoutEffect: 画面更新前に実行（同期）
  useLayoutEffect(() => {
    console.log('画面更新前');
  });
}
```
## why
