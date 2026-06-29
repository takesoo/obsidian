---
tags:
  - nodejs
  - npm
---
## what
- [[パッケージ管理システム|パッケージマネージャ]]のバージョンを管理するマネージャー
## how
1. corepackを有効化する
```bash
corepack enable
```
2. `package.json`で指定する。
```json
// package.json
{
	"name": "my-project",
	"version": "1.0.0",
	"packageManager": "pnpm@10.0.0"
}
```
3. `pnpm`コマンドが自動で↑のバージョンになる