---
aliases:
  - fs
  - node:fs
tags:
  - nodejs
  - nodejs/module
---
[[ファイルシステム]]からfileの読み書きを提供するモジュール。
同期APIと非同期APIの両方が提供されている。ただし、非同期処理を実装する場合は[[node fs promises|node:fs/promises]]を使用するのが一般的。async/await構文も使用できる。
```typescript
import fs from 'fs'

function main() {
	fs.readdirSync(directory);
	fs.readFileSync(filePath, 'utf8');
}
```
