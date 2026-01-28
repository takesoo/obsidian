---
aliases:
  - リゾルバー
---
## what
- パスを解決するプログラム
```js
import Button from './Button'
/**
 * './Button'というファイルはあるか？→ない
 * Button.js? -> no
 * Button.tsx? -> yes
 * so, './Button' means `/src/components/Button.tsx`.
 */
```