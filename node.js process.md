---
tags:
  - nodejs/module
aliases:
  - node:process
---
実行中の[[プロセス]]の情報と操作を提供するオブジェクト
```js
import process from 'process';

process.on('beforeExit', (code) => {
  console.log('Process beforeExit event with code: ', code);
});

process.on('exit', (code) => {
  console.log('Process exit event with code: ', code);
});

console.log('This message is displayed first.');

// Prints:
// This message is displayed first.
// Process beforeExit event with code: 0
// Process exit event with code: 0
```