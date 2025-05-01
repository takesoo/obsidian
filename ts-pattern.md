---
tags:
  - npm
---
パターンマッチングライブラリ
```js
import { match, P } from 'ts-pattern';

type Data =
  | { type: 'text'; content: string }
  | { type: 'img'; src: string };

type Result =
  | { type: 'ok'; data: Data }
  | { type: 'error'; error: Error };

const result: Result = ...;

const html = match(result)
  .with({ type: 'error' }, () => <p>Oups! An error occured</p>)
  .with({ type: 'ok', data: { type: 'text' } }, (res) => <p>{res.data.content}</p>)
  .with({ type: 'ok', data: { type: 'img', src: P.select() } }, (src) => <img src={src} />)
  .exhaustive();
```

[gvergnaud/ts-pattern: 🎨 The exhaustive Pattern Matching library for TypeScript, with smart type inference.](https://github.com/gvergnaud/ts-pattern)
[ts-patternでTypeScriptにパターンマッチングを持ち込み、より型安全な世界へ](https://zenn.dev/aki202/articles/5d725c080640f9)
