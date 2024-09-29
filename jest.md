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