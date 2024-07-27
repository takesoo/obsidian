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
};
type Human = {
	age: number;
	name: string;
};

/**
	型引数を使った型定義
	型引数を持つ型をジェネリック型と呼ぶ
*/
type Family<Parent, Child> = {
	mother: Parent;
	father: Parent;
	child: Child;
};
const obj: Family<number, string> = {
	mother: 0,
	father: 100,
	childer: "1000"
}

/**
	extendsによる型定義
	P extends T: PはTの部分型でなければならない
*/
type HasName = {
	name: string;
};
type Family<Parent extends HasName, Child extends HasName> = {
	mother: Parent;
	father: Patent;
	child: Child;
};
type T = Family<number, string>; // 型エラー

```
### 配列
TypeScriptでは、配列はオブジェクトの一種。
```ts
const arr1: number[] = [4, 5, 6];
const arr2 = [1, 2, 3, ...arr1];
const arr3 = Array<{
	name: string;
}> = [
	{ name: "山田" },
]
const arr4: readonly number[] = [1, 10, 100] // 読み取り専用
for(const elm of arr4) {
	...
}

/**
	タプル型
	要素数が固定された配列型
	TypeScriptではタプル型は配列型の一種
	関数の可変長引数などの型定義で使用する
*/
let tuple: [sting, number] = ["foo", 0]
type User = [name: string, age: number] // ラベル付きタプル型
const yamada: User = ["Yamada", 34]
console.log(yamada[0]) // =>"Yamada"
```
### 分割代入
```ts
const obj = {
	foo: 123,
	bar: "Hello, world!"
};
const { foo, bar } = obj;

const nested = {
	num: 123,
	obj: {
		foo: "hello",
		bar: "world"
	}
};
const { num, obj: { foo } } = nested;
console.log(foo); // =>"hello"

const arr = [1, 2, 3, 4];
const [first, second, third] = arr;
console.log(third); // =>3

const { foo: num, ...restObj } = obj; // obj.fooを変数numに代入する
console.log(num); // =>123
console.log(restObj); // =>{bar:"Hello, world!"}

const arr = [1,2,3,4];
const [first, second, ...rest] = arr;
console.log(rest); // =>[3,4]
```
### 組み込みオブジェクト
```ts
// Date
const d = new Date();
d.getFullYear();
d.getMonth();
d.getTime(); // UNIX時間に変換
Date.now(); // 現在時刻
// Dateは使いにくいと言われている。Temporalに置き換わる可能性もある。

// RegEXp
const r = /ab+c/;
console.log(r.test("abbbbc")); // =>true
console.log("Hello, abbbc world! abbbbc".replace(r, "foobar")); // =>"Hello, foobar world! abbbbc"

const result = "Hello, abbbbbbc world! abc".match(/a(b+)c/); // ()はキャプチャリンググループ
if (result !== null) { // マッチしない場合resultはnullになる。nullガードしないとコンパイルエラーになる
  console.log(result[0]); // =>"abbbbbbc" result[0]は正規表現にマッチした部分文字列。 
  console.log(result[1]); // =>"bbbbbb" result[1]はキャプチャリンググループにマッチした部分文字列
}

const result = "Hello, abbbbbbc world! abc".match(/a(?<worldName>b+)c/); // (?<groupName>...) 名前付きキャプチャリンググループ
if (result !== null) {
  console.log(result.groups); // =>{ "worldName": "bbbbbb" }
}

// Map
const map: Map<string, number> = new Map(); // Map<key, value>
map.set("foo", 1234);
console.log(map.get("foo")); // =>1234
console.log(map.get("bar")); // =>undefined

// Set
const set: Set<string> = new Set();
set.add("foo");
```
## 関数
```ts
// function 関数名(引数: 引数の型): 戻り値の型 {
function range(min: number, max: number): number[] {
	const result = [];
	for (let i = min; i <= max; i++) {
		result.push(i);
	}
	return result;
}

// 戻り値のない関数はvoid
function helloWorldNTimes(n: number): void {
	if (n < 0) {
		return; //return単独の場合もvoidとなる
	}
	for (let i = 0; i < n; i++) {
		console.log("Hello world!");
	}
}

/**
	関数式による定義
*/
type Human = {
	height: number;
	wight: number;
};
const = calcBMI = function(human: Human): number {
	return human.weight / human.height ** 2;
};
// 引数で分割代入をする場合
const = calcBMI = function({ height, wight }: Human): number {...
```
