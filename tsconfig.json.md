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

## strictオプション
strict系コンパイラオプションをまとめて有効にする
原則有効化することを推奨
```json
{
	"compilerOptions": {
		"strict": true
	}
}
```
### strict系コンパイラオプション
今後さらに追加されることもある
- noImplicitAny: 暗黙的にany型と推論することを禁止する。JSからの移行の際には一旦外すこともある。
- noImplicitThis
- alwaysStrict
- strictBindCallApply
- strictNullChecks: nullとundefinedをチェックする
- strictFunctionTypes
- strictPropertyInitialization
- useUnknownInCatchVariables
## noUncheckedIndexedAccess
[[インデックスシグネチャ]]を通じたプロパティアクセスで得られる型は常にundefinedとのユニオン型になる
デフォルトのtsconfig.jsonでは有効になっていないが、新規開発する際には有効化を推奨する。
```json
{
	"compilerOptions": {
		"noUncheckedIndexedAccess": true,
	}
}
```
```ts
type PriceData = {
	[key: string]: number;
}
const data: PriceData = {
	apple: 220,
	coffee: 120,
	bento: 500,
};

const applePrice = data.apple; // number | undefined

const arr = [1, 2, 3];
const val = arr[0]; // number | undefined
```
### exactOptionalPropertyTypes
オプショナルプロパティに対してundefinedを代入できなくなる。そのため、省略可能なプロパティの場合はオプショナルプロパティで定義し、undefinedも代入可能なプロパティの場合はユニオン型で定義しなければならなくなる。
```json
{
	"compilerOptions": {
		"exactOptionalPropertyTypes": true
	}
}
```
```ts
type Human = {
	name: string;
	age: number | undefined;
	job?: string;
}

const john: Human = {
	name: "John Smith",
	age: undefined,
	// error: Type 'undefined' is not assignable to type 'string'.
	job: undefined,
}

```