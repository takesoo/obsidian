---
tags:
  - TypeScript
---
[[TypeScript]]の設定ファイル
`npx tsc --init`で作成できる
コンパイルの設定などが記述できる

## include
どのファイルをコンパイル対象とするか
[[globパターン]]で記述する
```json
{
	"include": ["./src/**/*.ts"] // tsconfig.jsonから見て./src以下のすべての.tsファイルを対象とする
}
```
## exclude
includeで指定されたファイル群から一部のファイルを除外する
```json
{
	"include": ["./src/**/*.ts"],
	"exclude": ["./src/__tests__/**/*.ts"] // src/__tests__/以下の.tsファイルはコンパイル対象から除外する
}
```

