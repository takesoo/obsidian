---
tags:
  - npm
---
- gitでステージしたファイルに対してリントを走らせるパッケージ
- [[husky]]などプリコミットライブラリと一緒に使う
- 設定方法はいくつかある
	- package.jsonに設定する方法
	- `.lintstagedrc.json|yaml|yml`を設定する
	- `.lintstagedrc.mjs`または`lint-staged.config.mjs`に[[ESM]]形式で設定する
	- `.lintstagedrc.cjs`または`lint-staged.config.cjs`に[[CommonJS]]形式で設定する
	- 他にもあるが、jsで書くのが一番
```js
// lint-staged.config.mjs

import path from 'path';

const buildEslintCommand = (filenames) => {
  return `next lint --fix --file ${filenames
    .filter((f) => f.includes('/src/'))
    .map((f) => path.relative(process.cwd(), f))
    .join(' --file ')}`;
};

const config = {
  '*.{ts,tsx}': [buildEslintCommand, "bash -c 'yarn check-types'"],
};

export default config;
```
