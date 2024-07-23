```js
const greeting: string = "Hello, "
const target: string = "world!"
console.log(greeting + target);
// "Hello, world!"
```

## プリミティブ
### 文字列
- `const str: string = "Hello, world!";`
- バッククオートで囲う文字列はテンプレートリテラルという
### 数値
- `const num: number = 123;`
- 演算子を使って計算ができる
- 整数と少数の区別がない。どちらもnumber型で許容できる
- 53ビットの精度
### 真偽値
- `const isTrue: boolean = false;`
### 任意精度整数(BigInt)
- `const bignum: bigint = 123np;`
- number型で扱えない桁数の時に使用する
- 整数の後にnを書くとbigintリテラル
- number型とbigint型を計算することはできない
### nullとundefined
- TypeScriptではnullとundefined両方存在する。
- undefinedの方がサポートが厚いのでundefinedを使う方が安心
### symbol


