---
tags:
  - テスティングフレームワーク
  - JavaScript
---
- JavaScriptのテスティングフレームワーク
- [[スナップショットテスト]]ができる

```bash
pnpm add --save-dev jest # jsの場合
pnpm add --save-dev ts-jest # tsの場合
```

```js
const sum = require('.sum');

test('adds 1 + 2 to equal 3', () => {
	expect(sum(1, 2)).toBe(3);
});
```

```bash
jest my-test --notify --config=config.json
```

```bash
# テストもtsで書きたいとき
pnpm add --save-dev @jest/globals
# or
pnpm add --save-dev @types/jest
```
## jest.config.ts
## jest.setup.ts
テスト実行前に特定の設定やグローバルな準備を行うためのファイル。
## Mock
```js
const forEach = require('./forEach');

const mockCallback = jest.fn(x => 42 + x);

test('forEach mock function', () => {
  forEach([0, 1], mockCallback);

  // The mock function was called twice
  expect(mockCallback.mock.calls).toHaveLength(2);

  // The first argument of the first call to the function was 0
  expect(mockCallback.mock.calls[0][0]).toBe(0);

  // The first argument of the second call to the function was 1
  expect(mockCallback.mock.calls[1][0]).toBe(1);

  // The return value of the first call to the function was 42
  expect(mockCallback.mock.results[0].value).toBe(42);
});

// 関数のモック
const myMock = jest.fn();
console.log(myMock());
// > undefined

myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);

console.log(myMock(), myMock(), myMock(), myMock());
// > 10, 'x', true, true

// モジュールのモック
import axios from 'axios';
import Users from './users';

jest.mock('axios');

test('should fetch users', () => {
  const users = [{name: 'Bob'}];
  const resp = {data: users};
  axios.get.mockResolvedValue(resp);

  // or you could use the following depending on your use case:
  // axios.get.mockImplementation(() => Promise.resolve(resp))

  return Users.all().then(data => expect(data).toEqual(users));
});
```