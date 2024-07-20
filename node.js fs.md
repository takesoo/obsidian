---
aliases:
  - fs
tags:
  - nodejs
  - nodejs/module
---
[[ファイルシステム]]からfileの読み書きを提供するモジュール
```typescript
import fs from 'fs'

function main() {
	fs.readdirSync(directory);
	fs.readFileSync(filePath, 'utf8');
}
```
