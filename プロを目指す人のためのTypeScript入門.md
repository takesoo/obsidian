---
tags:
  - book
---
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
```
### 部分型関係
```ts
/**
	部分型(subtyping relation)
	2つの型の互換性を表す概念
	次の条件を満たす時、型Sは型Tの部分型となる
		1. Tが持つプロパティはすべてSにも存在する
		2. Sのプロパティの型はTのプロパティの型の部分型（または同じ型）である
	S型の値はT型の値の代わりになる（S型の値はT型の値に代入できる。）
	Human型はAnimal型の部分型
*/
type Anymal = {
	age: number;
};
type Human = {
	age: number;
	name: string;
};
const human: Human = {
	age: 26,
	name: "uhyo",
};
const animal: Animal = human; // HumanはAnimalの部分型であることから、Human型の値であるhumanはAnimal型でもあるので、代入できる。

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
	extendsを使った型定義
	P extends T: PはTの部分型でなければならないという制約を付与する
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
### 関数の作り方
```ts
/**
	関数宣言による定義
	function 関数名(引数: 引数の型): 戻り値の型 {
	関数宣言より前に呼び出しても（巻き上げしても）エラーにならない。
*/
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
	const 関数名 = function(引数リスト): 戻り値の型 {
	変数宣言なので巻き上げをするとエラーになる
*/
type Human = {
	height: number;
	wight: number;
};
const calcBMI = function(human: Human): number {
	return human.weight / human.height ** 2;
};
// 引数で分割代入をする場合
const calcBMI = function({ height, wight }: Human): number {
	return weight / height ** 2;
};

/**
	アロー関数による定義
	const 関数名 = (引数リスト): 戻り値の型 => {
*/
const calcBMI = ({ height, weight }: Human):number => {
	return weight / height ** 2;
};

// 省略形。計算式が一つだけの場合に使える。
const calcBMI = ({ height, weight }: Human):number => weight / height ** 2;

// オブジェクトを返す場合はオブジェクトリテラルを()で囲う
type ReturnObj = {
	bmi: number
};
const calcBMIObject = ({ height, weight }):ReturnObj => ({
	bmi: weight / height ** 2
});

/**
	メソッド記法
	オブジェクトの中でメソッドとして関数を定義する方法
*/
const obj = {
	double(num: number): number {
		return num * 2;
	},
	double2: (num: number): number => num * 2,
}
console.log(obj.double(100)); // =>200

/**
	可変長引数(rest引数)
	型は必ず配列型（またはタプル型）
*/
const sum = (...args: number[]): number => {
	let result = 0;
	for (const num of args) {
		result += num;
	}
	return result;
}
console.log(sum(1, 10, 100)); // =>111

/**
	スプレッド構文
*/
const nums = [1, 2, 3, 4, 5];
console.log(sum(...nums)); // =>15

/**
	オプショナル引数
	初期値としてundefinedが入る。つまりupperの型は boolean | undefined。
*/
const toLowerOrUpper = (str: string, upper?: boolean): string => {
	if (upper) {
		return str.toUpperCase();
	}
	return str.toLowerCase();
};

// 引数の初期値設定。この場合upperの方はboolean。
const toLowerOrUpper = (str: string, upper: boolean = false): string => {
...
}

/**
	コールバック関数
*/
type User = { name: string; age: number };
const users: User[] = [
	{ name: "uhyo", age: 26 },
	{ name: "John Smith", age: 15 }
]

// 引数に渡している関数のことをコールバック関数と呼ぶ
// コールバック関数を引数として受け取る関数を高階関数(higher-order function)と呼ぶ
const names = users.map((u: User): string => u.name);
console.log(names); // =>["uhyo", "John Smith"]
```
### 関数の型
```ts
/**
	関数型はアロー関数のように定義する
	このとき、引数名は型チェックに影響しない。つまり関数定義の際に使用する引数名と型の引数名が異なっていても問題がない。エディタの補完の際にわかりやすいようするために命名する。
*/
type F = (repeatNum: number) => string;
const xRepeat: F = (num: number): string => "x".repeat(num);

/**
	戻り値の型注釈は省略できる
	↓の場合、型推論によって戻り値はstringと判定される
	基本的に省略しない方が堅牢なシステムになる。省略すると関数の実装にミスがあったら、呼び出し側でコンパイルエラーとなってしまい、原因箇所が隠蔽されてしまうため。
*/
const xRepeat(num: number) => "x".repeat(num)

/**
	引数の型注釈は省略できる
	基本的に省略してOK
	↓の場合、引数numの方はnumberであることが型Fから推論される。
*/
type F = (arg: number) => string;
const xRepeat: F = (num) => "x".repeat(num);

// コールバック関数の引数の型も省略できる。↓の場合、引数xの型はnumberであると推論される
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const arr2 = nums.filter((x) => x % 3 === 0);

// ↓の場合も型Greetableから引数strの型はstringと推論される。
type Greetable = {
	greet: (str: string) => string;
}
const obj: Greetable = {
	greet: (str) => `Hello, ${str}!`
};

```
### 関数型の部分型関係
```ts
/*
	戻り値の型による部分型関係
	SがTの部分型であれば、「Sを返す関数」の型は「Tを返す関数」の型の部分型である。
	HasNameAndAgeはHasNameの部分型なので、関数fromAgeは関数fの部分型であり、変数fに変数fromAgeを代入できる。
	ただし、引数の型が一致する場合に限る
*/
type HasName = {
	name: string;
}
type HasNameAndAge = {
	name: string;
	age: number;
}

const fromAge = (age: number): HasNameAndAge => ({
	name: "John Smith",
	age,
});
const f: (age: number) => HasName = fromAge;
// fの実際の戻り値はHasNameAndAge型が戻ることに注意。
const obj: HasName = f(100); // =>{ name: "John Smith", age: 100 }

/*
	引数の型による部分型関係
	SがTの部分型であれば、「Tを引数に受け取る関数」の型は「Sを引数に受け取る関数」の型の部分型となる
	HasNameAndAgeはHasNameの部分型なので、関数showNameの型は関数gの型の部分型となり、変数gに変数showNameを代入できる。
*/
const showName = (obj: HasName) => {
	console.log(obj.name);
}
const g: (obj: HasNameAndAge) => void = showName;
g({ name: "uhyo", age: 26 }); // =>"uhyo"

/*
	引数の数による部分型関係
	関数型Fの引数リストの末尾に新たな引数を追加して関数型Gを作った場合、FはGの部分型となる
	↓の場合、UnaryFuncはBinaryFuncの部分型
*/
type UnaryFunc = (arg: number) => number;
type BinaryFunc = (left: number, right: number) => number;
const double: UnaryFunc = arg => arg * 2;
const add: BinaryFunc = (left, right) => left + right;
const bin: BinaryFunc = double; // UnaryFuncをBinaryFuncとして扱うことができる
console.log(bin(10, 100)); // =>20 第二引数は無視される
```
### ジェネリクス
```ts
/*
	ジェネリクス(generics)
	型引数を受け取る関数(ジェネリック関数)を作る機能のこと
	型引数に何を当てはめるかは関数を使う側で決めることができる
*/
function repeat<T>(element: T, length: number): T[] {
	const result: [] = [];
	for (let i = 0, i < lenght; i++) {
		result.push(element);
	}
	return result;
}
console.log(repeat<string>("a", 5)); // =>["a", "a", "a", "a", "a"]
console.log(repeat<number>(123, 3)); // =>[123, 123, 123]

// function関数式
const repeat = function<T>(element: T, length: number): T[] {
...
}

// アロー関数
const repeat = <T>(element: T, length: number): T[] => {
...
}

// メソッド記法
const utils = {
	repeat<T>(element: T, length: number): T[] {
	...
	}
}

// extendsを使った定義
const repeat2 = <T extends {
	name: string;
}>(element: T, length: number): T[] => {
...
}

type HasNameAndAge = {
	name: string;
	age: number;
}

console.log(repeat2<HasNameAndAge>({ name: "uhyo", age: 26 }, 3));
// =>[{ name: "uhyo", age: 26 }, { name: "uhyo", age: 26 }, { name: "uhyo", age: 26 }]
console.log(repeat2<string>("a", 5)); // コンパイルエラー

// 関数の型引数は省略できる。型引数Tは引数の型から推論される。
console.log(repeat("a", 5)); // =>["a", "a", "a", "a", "a"]

// 型定義する場合
type Func = <T>(arg: T, num: number) => T[];
const repeat: Func = (element, lenght) => {
...
};
```
## クラス
### クラスの宣言と使用
```ts
class User {
	name: string = ""; // プロパティ宣言には初期値が必要
	age?: number; // オプショナルプロパティは初期値不要
	readonly isMarried: boolean = false; // 読み取り専用
}

const uhyo = new User();
console.log(uhyo); // =>{ name: "", age: undefined, isMarried: false }
uhyo.age = 26;
console.log(uhyo); // =>{ name: "", age: 26, isMarried: false }
uhyo.isMarried = true; // Cannot assign to 'isMarried' because it is read-only property.

/*
	メソッドの宣言
*/
class User {
	name: string = "";
	age: number = 0;

	isAdult(): boolean {
		return this.age >= 20;
	}

	setAge(newAge: number) {
		this.age = newAge;
	}
}

const uhyo = new User();
console.log(uhyo.isAdult()); // =>false
uhyo.setAge(26);
console.log(uhyo.isAdult()); // =>true

/*
	コンストラクタ
	コンストラクタがある場合はプロパティの初期値は不要
*/
class User {
	name: string;
	age: number;

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}

	isAdult(): boolean {
		return this.age >= 20;
	}
}

const uhyo = new User("uhyo", 26);
console.log(uhyo.name); // =>"uhyo"
console.log(uhyo.isAdult()); // =>true

/*
	静的プロパティ(static property)と静的メソッド(static method)
	rubyでいうクラスメソッド
*/
class User {
	static adminName: string = "uhyo";
	static getAdminUser() {
		return new User(User.adminName, 26);
	}

	name: string;
	age: number;

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}

	isAdult(): boolean {
		return this.age >= 20;
	}
}

console.log(User.adminName); // =>"uhyo"
const admin = User.getAdminUser();
console.log(admin.age); // =>26
console.log(admin.isAdult()); // =>true

/*
	アクセシビリティ修飾子(accessibility modifier)
	public
	protected
	private
*/
class User {
	name: string;
	private age: number; // #age: number;とも書ける

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}

	public isAdult(): boolean {
		return this.age > 20;
	}
}

const uhyo = new User("uhyo", 26);
console.log(uhyo.name); // =>"uhyo"
console.log(uhyo.isAdult()); // =>true
console.log(uhyo.age); // error: Property 'age' is private and only accessibile with in class 'User'
```
### クラスの型
```ts
/*
	クラス宣言はインスタンスの型を作る
*/
class User {
	name: string = "";
	age: number = 0;

	isAdult(): boolean {
		return this.age >= 20;
	}
}

const uhyo = new User();
console.log(uhyo instanceof User); // =>true
console.log(typeof(uhyo)); // =>User

// ↓はUser型のオブジェクトを作成
const john: User = {
	name: "John Smith",
	age: 15,
	isAdult: () => true
}
console.log(john instanceof User); // =>false Userのインスタンスではない(newでイニシャライズしてない)ので
```
### クラスの継承
```ts
/*
	クラスの継承
	子クラスのインスタンスは親クラスのインスタンスの部分型でなければならない
*/
class User {
	name: string;
	// protected修飾子 外部からアクセスできないが子クラスからはアクセスできる
	//　子クラスがロジックを拡張することを許す時に使う
	// 基本的にprivateを使用する方が安全
	protected age: number;

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}

	public isAdult(): boolean {
		return this.age >= 20;
	}
}

class PremiumUser extends User {
	rank: number = 1;

	constructor(name: string, age: number, rank: number) {
		super(name, age);
		this.rank = rank;
	}

	// tsconfig.jsonでnoImplicitOverrideオプションを有効にすると、オーバーライドする時にoverride修飾子が必要になる
	public override isAdult(): boolean {
		return this.age >= 18;
	}
}

console.log(new PremiumUser("uhyo", 26).age); // Property 'age' is protected and only accessible within 'User' and its subclasses.

/*
	implementsキーワード
	class A implements B: Aの型はBの部分型である
*/
type HasName = {
	name: string;
}

// Class 'User' incorrectly implements interface 'HasName'.
// Property 'name' is missing in type 'User' but required in type 'HasName'.
class User implements HasName {
	#age: number;

	constructor(age: number) {
		this.#age = age;
	}
}
```
### this
```ts
/*
	TypeScriptのthisは呼び出し方によってはエラーになる
	thisを使うオブジェクトのメソッドは原則としてメソッド記法で呼び出すべき
*/
class User {
	name: string;
	#age: number;
	
	constructor(name: string, age: number) {
		this.name = name;
		this.#age = age;
	}

	public isAdult(): boolean {
		return this.#age >= 20;
	}
}

const uhyo = new User("uhyo", 26);
console.log(uhyo.isAdult()); // =>true
const isAdult = uhyo.isAdult; // isAdult関数オブジェクトをisAdult変数に代入
// runtime error: attempted to get field on non-instance
// オブジェクト.関数()の形式で呼び出していないので、thisがundefinedになってしまうため
console.log(isAdult());

/*
	アロー関数は外側の関数からthisを受け継ぐ（アロー関数は自分自身のthisを持たない）
*/
class User {
...
	public filterOlder(users: readonly User[]): User[] {
		return users.filter(u => u.#age > this.#age); // このthisはfilterOlderの呼び出し元を受け継ぐ
	}
}

const uhyo = new User("uhyo", 26);
const john = new User("john Smith", 15);
const bob = new User("Bob", 40);
const older = uhyo.filterOlder([john, bob]); // filterOlder内のコールバック関数内のthisはuhyoになる
console.log(older); // =>[User { name: "Bob" }]
```
### 例外処理
```ts
/*
	throw文とErrorオブジェクト
	ランタイムエラーが発生し、プログラムの実行が中断される
*/
throw new Error("エラーが発生しました！");

/*
	file///path/to/project/src/index.js:1
		throw error = new Error("エラーが発生しました！");
						  ^
	Error: エラーが発生しました！                           // エラーメッセージ
		at file///path/to/project/src/index.js:1:22     // スタックトレース
*/

/*
	try-catch文
	catchでエラーを握りつぶすのはアンチパターンなので注意する
	処理の失敗を返す場合は、基本的にthrowで例外を発生させるよりもfalsyな値を返す方がシンプルに書ける
*/
try {
	throw new Error("エラーが発生しました！");
} catch (err) {
	console.log(err); // => エラーメッセージとスタックトレースが表示される
} finally {
...
}
```
オブジェクト指向プログラミングではクラスに振る舞いを持たせるが、TypeScriptにおいてはあまりメリットにならない。データと値を分離した方が便利なことが多くある。コードのminimizationの観点からも関数はメソッドではなく独立した関数にした方が有利とされている。
クラスのメソッドが一つしかないならクロージャ関数で実装するという方法もある。
## 高度な型
### ユニオン型とインターセクション型
```ts
/*
	ユニオン型
	T型またはU型
*/
type Animal = {
	species: string;
	age: string;
}
type Human = {
	name: string;
	age: number;
}
type User = Animal | Human;

// Animal型オブジェクトなのでUser型に代入できる
const tama: User = {
	species: "Felis silvestris catus",
	age: "永遠の17歳"
};
// Human型オブジェクトなのでUser型に代入できる
const uhyo: User = {
	name: "uhyo",
	age: 26
};

// userがAnimal型の場合にはnameプロパティを持たないため、エラーになる
function getName(user: User): string {
	// error: Property 'name' does not exist on type 'User'.
	// Property 'name' does not exist on type 'Animal'.
	return user.name;
}

// ageはstring | number型になる
function showAge(user: User) {
	const age = user.age;
	console.log(age);
}

// resultはstring | number型になる
type MysteryFunc = 
	| ((str: string) => string)
	| ((srt: string) => number);

function useFunc(func: MysteryFunc) {
	const result = func("uhyo");
	console.log(result);
}

/*
	インターセクション型
	T型かつU型
*/
type Animal = {
	species: string;
	age: number;
}
// HumanはAnimalの部分型になる
type Human = Animal & {
	name: string;
}

const tama: Animal = {
	species: "Felis silvestris catus",
	age: 3
}
const uhyo: Human = {
	species: "Homo sapiens sapiens",
	age: 26,
	name: "uhyo"
}

/*
	ユニオン型とインターセクション型の関係
*/
type Human = { name: string };
type Animal = { species: string };

function getName(human: Human) {
	return human.name;
}
function getSpecies(animal: animal) {
	return animal.species;
}

// mysteryFuncの型は ((human: Human) => string | (animal: Animal) => string)
const mysteryFunc = Math.randomw() < 0.5 ? getName : getSpecies;

// mysteryFuncにHuman型を渡すとエラー
// error: Argument of type 'Human' is not assignable to parameter of type 'Human & Animal'.
// Property 'species' is missing in type 'Human' but required in type 'Animal'.
mysteryFunc(uhyo);

// mysteryFuncにAnimal型を渡すとエラー
// error: Argument of type 'Animal' is not assignable to parameter of type 'Human & Animal'.
// Property 'name' is missing in type 'Animal' but required in type 'Animal'.
mysteryFunc(cat);

// Human & Animal型を渡すと呼び出せる
const taro: Human & Animal = {
	name: "uhyo",
	species: "Homo sapiens sapiens"
}
mysteryFunc(taro);

/*
	オプショナルプロパティの型
*/
type Human = {
	name: string;
	age?: number; // number | undefined
}

const john: Human = {
	name: "John Smith",
	// tsconfig.jsonでexactOptionalPropertyTypesを有効にするとコンパイルエラーを発生させられる。堅牢になるので有効化が推奨される。
	// error: Type 'undefined' is not assignable to type 'number'.
	age: undefined
}

/*
	プロパティアクセスのオプショナルチェイニング
	obj?.prop
	オブジェクトがnullやundefinedの場合はundefinedを返す
*/

type Human = {
	name: string;
	age: number;
}

function useMaybeHuman(human: Human | undefined) {
	const age = human?.age;
...
}

/*
	関数呼び出しのオプショナルチェイン
	func?.()
*/
type GetTimeFunc = () => Date;

function useTime(getTimeFunc: GetTimeFunc | undefined) {
	const timeOrUndefined = getTimeFunc?.();　// Date | undefined
}


```