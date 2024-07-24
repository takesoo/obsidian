```ts
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

## 型変換
```ts
// 暗黙的な型変換
const arg = '123';
console.log(arg + 1000); // 1000が文字列として処理される
// => '1231000'

// 明示的な型変換
const num = Number(arg);
console.log(num + 1000);
// => 1123
```
falseになる値: `0, 0n, NaN, "", null undefined`

## オブジェクト
```ts
const propName = "hoge"
const obj = {
	foo: 123,
	"bar": 'Hello, world!',
	"foo bar": -500,
	1: "one",
	[propName]: 123
};
console.log(obj.foo);
// =>123
console.log(obj.bar);
// =>'Hello, world!'
console.log(obj["foo bar"]);
// =>-500
console.log(obj[1]);
// =>"one"
console.log(obj.hoge);
// =>123
obj.foo = 234;
console.log(obj.foo)
// =>234
const obj2 = {
	...obj // スプレッド構文。値のコピーであり、ポインタは別。
	foo: 1234
};
console.log(obj2);
/**
=> {
	foo: 1234, あとに書かれた方に上書きされる
	bar: 'Hello, world!',
	foo bar: -500,
	1: "one",
	hoge: 123
}
*/
```
```ts
const obj: {
	foo: number;
	bar: string;
} = {
	foo: 123,
	bar: "Hello, world!"
};

// type文による型定義
type FooBarObj = {
	foo: number;
	bar?: string; // オプショナル。string | undefinedと同じ
	readonly hoge: number // 読み取り専用。再代入できない。
};
const obj: FooBarObj = {
	foo: 123,
	bar: "Hello, world!"
};

/**
	interface宣言による型定義
	2014年以前はinterface宣言での型定義しかなかったために今も残っている記法
	基本的にtype文で代用可能なのでtype文を使用する
*/
interface FooBarObj {
	foo: number;
	bar: string;
}
const obj: FooBarObj = {
	foo: 0,
	bar: "one"
}

/**
	インデックスシグネチャ
*/
type PriceData = {
	[key: string]: number;
};
const data: PriceData = {
	apple: 220,
	coffee: 120,
	bento: 500
};
data.chicken = 250;

/**
	部分型
	Human型はAnimal型の部分型
*/
type Anymal = {
	age: number;
}
type Human = {
	age: number;
	name: string;
}

/**
	型引数を使った型定義
	型引数を持つ型をジェネリック型と呼ぶ
*/
type Family<Parent, Child> = {
	mother: Parent;
	father: Parent;
	child: Child;
}
const obj: Family<number, string> = {
	
}
```
