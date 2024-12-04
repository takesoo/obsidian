---
finished reading: false
favorite: false
tags:
  - clippings
---
# Re: type-challengesから始めるTypeScript実践演習 - 初級編
  #ReadItLater 
 #ReadableArticle

## articleURL
https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09

## siteName
Zenn

## date
2024-11-17

## articleContent
type-challengesは型システムの仕組みをより深く理解し、独自のユーティリティを記述したり、あるいは単に型パズルを楽しんだりできるOSSプロジェクトです。

とはいえ、いきなり`type-challenges`に挑むと挫折します。少なくとも私がそうでした。

なので、この記事ではまず最初にtype-challengesを解くために必要なTypeScriptの知識を解説します。

その後、実際にtype-challengesの問題を解いてみることで、座学→実践という形式でTypeScriptの型システムを学ぶことを目指します。

この記事は「一度はTypeScriptの[公式ドキュメント](https://www.typescriptlang.org/)や[サバイバルTypeScript](https://typescriptbook.jp/)を通して読んだけど、実際にコードを書こうとすると手が動かん…」[\[1\]](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fn-9d12-1)という方向けです。[\[2\]](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fn-9d12-2) なので`Re:`です。

今回は初級編です。今後も何回かに分けて中級編、上級編と続けていく予定~は未定~です。

## 初級編を解くために必要な知識

## Conditional Type Constraints

公式ドキュメントを読むのが一番いいのですが、よく見かける`extends`というヤツです。とにかくよく使います。カービィの吸い込みくらい基本技です。

使い方は`Conditional Type Constraints`という名前通り、**型を条件分岐して決めたい**場合に使います。例えばこんな感じ。

```
type ConditionalString<T> = T extends string ? string : T
type IdString = ConditionalString<"123456"> // string
type IdNumber = ConditionalString<123456> // 123456
```

ここでは`T`が`string`型であればそれを返し、そうでなければ元の`T`の型を返しています。

### Distributive Conditional Types

`extends`の派生…というより特性ですね。これも公式ドキュメントがわかりやすい。

日本語にすると`条件型の分配法則`でしょうか。どういうことかというと、**UNION型に対する条件型の判定は、UNION型の各型に対して適用され、結果もまたUNION型として生成される**ということです。以下のサンプルを見てください。

コードは公式ドキュメントのそれをほぼそのままお借りしています。

```
type ToArray<T> = T extends string | number ? T[] : never;
type StrArrOrNumArr = ToArray<string | number | symbol>; // string[] | number[]
```

この結果はつまり、

-   `string | number | symbol`のそれぞれに対して`T extends string | number ? T[] : never`が適用されている
-   条件に該当した`string`, `number`の結果が`string[] | number[]`となった
-   `never`となったものはUNION型の一部として扱われていない

ということを表しています。

## Inferring Within Conditional Types

こっちも公式ドキュメントを読むのが一番手っ取り早いです。`Inferring Within Conditional Types`は`infer`というヤツです。

使い方はこっちも名前通り、`型の条件分岐内部で型を推論してほしい`場合[\[3\]](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fn-9d12-3)に使います。

例えばこんな感じ。

こっちの例は公式ドキュメントの例をほぼそのまま拝借しちゃいます。

```
type GetReturnType<T> = T extends (...args: any[]) => infer R
  ? R
  : never;

const func1 = () => "HelloWorld"
const func2 = (a: number, b:number) => a*b
type TypeFunc1 = GetReturnType<typeof func1> // string
type TypeFunc2 = GetReturnType<typeof func2> // number
type TypeFunc3 = GetReturnType<"123456"> // never
```

さっき覚えた通り、`extends`は条件分岐です。今回の条件は何かというと、`T`が関数型であればその戻り値の型を返し、そうでなければ`never`を返しています。

ここで登場しているのが`infer`で文字通りこれは推論です。この例の場合はサンプルコードから分かる通り、戻り値の型を推論しています。

## Keyof Type Operator

こいつもよく使います。`keyof`はオブジェクト型を受け取り、そのキーの文字列または数値リテラルの`UNION型`を生成します。

公式ドキュメントはこちら。

`keyof`のドキュメントは少しイメージが湧きにくかったので、こういう感じで確認してみます。

文字列リテラルのUNIONになる

```
interface Todo {
  title: string
  description: string
  completed: boolean
}

type KeyOfTodo = keyof Todo // "title" | "description" | "completed"

const title: KeyOfTodo = "title";
const description: KeyOfTodo = "description";
const completed: KeyOfTodo = "completed";

const date: KeyOfTodo = "date"; // error 
```

![](https://res.cloudinary.com/zenn/image/fetch/s--ZzIrkq0W--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/c24f20f53a4598cf6640b020.png%3Fsha%3Dff7ac1b3b1ae3bf0ca2350bc5138410985f467f4)

以下の例では`10`を扱えていることから、`0 | 1`という数値リテラルではなく、`number`として扱われていることがわかります。

numberになる

```
type Arrayish = { [n: number]: unknown };
const numberKeyObj:Arrayish  = {0: "hogehoge", 1: "fugafuga"}
const strKeyObj: Arrayish = {hoge: "hogehoge", fuga: "fugafuga"} // error

type Arr = keyof Arrayish;
const numberKey: Arr = 0
const numberKey2: Arr = 10
const stringKey: Arr = "hogehoge" // error
```

![](https://res.cloudinary.com/zenn/image/fetch/s--nVXcxdj2--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/0e6687a0928e099684ab0aa8.png%3Fsha%3Db3419a6704c7af9881668b3069de94349c36218e)

最後はこれ。

string | numberになる

```
type Mapish = { [k: string]: string };
const numberKeyObj:Mapish  = {0: "hogehoge", 1: "fugafuga"}
const strKeyObj: Mapish = {hoge: "hogehoge", fuga: "fugafuga"}

type M = keyof Mapish;
const numberKey: M = 0
const numberKey2: M = 10
const stringKey: M = "hogehoge"
```

![](https://res.cloudinary.com/zenn/image/fetch/s--2BlwTWxF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/e67762d87de9142cb7f0bf06.png%3Fsha%3Df38718230e2160074917b28b8012d5ab440d066b)

`[k: string]`にかかわらず、数値の`0`, `1`, `10`も扱えています。

これはドキュメントにあるとおり、JavaScript オブジェクト キーが常に文字列に強制変換されるため、 `obj[0]`は常に`obj["0"]`と同じになるためです。

## Mapped Types

これは通常、`[P in keyof T]: U`と表現されるものです。公式ドキュメントはこちら。

これはドキュメントそのまま見るのが一番わかりやすいですね。一応公式のサンプルコードをそのままお借りして載せておきます。

```
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type Features = {
  darkMode: () => void;
  newUserProfile: () => void;
};

/**
 * {
 *  darkMode: boolean;
 *  newUserProfile: boolean;
 *  }
 */
type FeatureOptions = OptionsFlags<Features>;
```

## Indexed Access Types

ほとんど文字通りなのですが、`T[Property]`のように指定することで、そのプロパティの型を取得することができます。

公式ドキュメントはこちら。

サンプルは自分が用意したやつで見てみます。

```
type Types = {
  str: string,
  nmb: number, // 48?
  bool: boolean
}

type STR = Types["str"] // string
type NMB = Types["nmb"] // number
type BOOL = Types["bool"] // boolean
```

また、こいつはインデックスにUNION型を指定すると、複数のキーに対応する型をUNIONで返してくれます。というわけでこいつは`keyof`と相性がよいです。

```
type Types = {
  str: string,
  nmb: number,
  bool: boolean
}

type STRorNMB = Types["str" | "nmb"] // string | number
type KeyofTypesType = Types[keyof Types] // string | number | boolean
```

### 配列に対して

`Indexed Access Types`にはもう一つ特性があります。これが全然直感的じゃないのですが、`typeof なんかの配列[number]`とすることで配列の要素の型を取得することができます。

この特性を使うことでこんなことができちゃいます。

```
const StylipS = ["ishihara", "ogura", "noto", "matsunaga"] as const;
type StylipSType = typeof StylipS // readonly ["ishihara", "ogura", "noto", "matsunaga"]
type StylipS = StylipSType[number] // "ishihara" | "ogura" | "noto" | "matsunaga"
```

「これがなんやねん」という感じですが、つまり`Mapped Types`と組み合わせて使う場合、オブジェクトと配列で手段が違うということです。

**オブジェクトは`[Property in keyof Type]`。配列は`type U = typeof T`としたうえで`[Property in U[number]]`としなければいけないということです。**

## PromiseLike

実務で使ったこと一度もないですが、初級編の問題を解くために必要なので書いてます。

これを使うと独自実装したクラスに対して`then()`メソッドを持たせ、それをコールする際に`async/await`を使うことができます。

詳しい説明はこちらの素晴らしい記事があるので、気になる方はお読みください。

ここでは`Promise`に対して`PromiseLike`というものが、標準ライブラリにこのように定義されていることだけ知っていればよいです。

```
interface Promise<T> {
  then<TResult1 = T, TResult2 = never>(
    onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null, 
    onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null
  ): Promise<TResult1 | TResult2>;
}

interface PromiseLike<T> {
  then<TResult1 = T, TResult2 = never>(
      onfulfilled?: (value: T) => TResult1 | PromiseLike<TResult1>,
      onrejected?: (reason: any) => TResult2 | PromiseLike<TResult2>
  ): PromiseLike<TResult1 | TResult2>;
}
```

## readonly unknown\[\] vs any\[\]

`PromiseLike`と同じく頭の片隅に置いておく必要がある話です。

### readonly unknown\[\]

読み取り専用のタプルまたは配列を許可します。`as const`で宣言した定数タプル（例えば、`const tuple = [1] as const`）は`readonly`属性を持ちます。

### any\[\]

読み取り専用でないタプルや配列型を許容しますが、`readonly`属性がついた配列やタプルとは互換性がありません。

## 問題と解法

初級編を解くために必要な道具はこれだけです(めっちゃ少ない！)

あとはこれを組み合わせることでtype-challenges(初級)の問題は解くことができます。

では、さっそく学んだ知識を馴染ませていきましょう。

## Pick

```
interface Todo {
  title: string
  description: string
  completed: boolean
}

type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
}
```

### 考え方

-   `MyPick<T, K>`の`K`は`T`のキーのみ受け入れられるように型を付けてあげれば良さそう
    -   `T`のプロパティたちは`keyof`で取り出せそう
    -   `keyof`のうちどのUNION型を許すか？という条件判定が必要そう。条件判定とくれば`extends`
-   絞った`K`を順に取り出して元の`T`プロパティの型を取り出せばよさそう
    -   `Mapped Types`と`Indexed Access Types`を組み合わせるとイイ感じにできそう

### 解答

```
type MyPick<T, K extends keyof T> = {
  [k in K]: T[k]
}
```

## Readonly

```
interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}

todo.title = "Hello" // Error: cannot reassign a readonly property
todo.description = "barFoo" // Error: cannot reassign a readonly property
```

### 考え方

極端な話、こういう`interface`を作り直してやればいい。

```
// オリジナル
interface Todo {
  title: string
  description: string
}

// 作り直したやつ
interface ReadOnlyTodo {
  readonly title: string
  readonly description: string
}
```

けど、これだと`Todo`専用というか、他にも`readonly`をつけたくなるたびに毎度`ReadOnlyxxxx`が出来ちゃう。

ただ、**全部のプロパティをくるくる回してreadonlyを付けつつ、元のプロパティに対する型を付けてあげる**って感じのことをしてあげればよくね？という感じではある。

ってことは、さっきの`Pick`の話を活用すればよさそう。つまり`Mapped Types`と`Indexed Access Types`を組み合わせるとよい。

### 解答例

```
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K]
}
```

## Tuple to Object

```
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const

type result = TupleToObject<typeof tuple> // expected { 'tesla': 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
```

### 考え方

今回はこれまでと違い`tuple`が対象のため、`[K in keyof T]`ができません。が、「配列で`Mapped Types`を使うにはどうすればよかったっけ…？」を思い出せば簡単に解けます。

### 解答例

```
type TupleToObject<T extends readonly (string | number | symbol)[]> = {
  [K in T[number]]: K
}
```

これでも問題のテストコードは通ってくれるのですが、`(string | number | symbol)`というのが妙に腑に落ちません。というか、`オブジェクトのプロパティのキーの型ですよ`っていうのを表現したい。

…そんなあなたに！

実はこんなものがあります。別解です。

```
type TupleToObject<T extends readonly PropertyKey[]> = {
  [K in T[number]]: K
}
```

これはTypeScriptが作ってくれている型です。`(string | number | symbol)`と置き換えられてるから察せるけど、こんな感じ。

```
declare type PropertyKey = string | number | symbol;
```

## First of Array

```
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type head1 = First<arr1> // expected to be 'a'
type head2 = First<arr2> // expected to be 3
```

### 考え方

さっきの`Tuple to Object`と同じように、配列に対する`Indexed Access Types`で型を取り出してあげればよさそう。

けどさっきとは違って`Mapped Types`で全部を回すんじゃなくて先頭だけを取りたい。

ってことは、`T[number]`じゃなくて`T[0]`って指定すればいけそう。

けど問題のテストケースを見てると、

```
Expect<Equal<First<[]>, never>>
```

っていうこいつが特殊なので、`T`の型が`[]`の時には`never`でそうじゃなければ`T[0]`っていう型の条件分岐が必要そう。こういうときは…？

### 解答例

```
type First<T extends any[]> = T extends [] ? never : T[0]
```

## Length of Tuple

```
type tesla = ['tesla', 'model 3', 'model X', 'model Y']
type spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT']

type teslaLength = Length<tesla>  // expected 4
type spaceXLength = Length<spaceX> // expected 5
```

### 考え方

イイ感じにステップアップ方式で問題順が並んでる気がしますね。また配列に対する`Indexed Access Types`を使ってあげる必要がありそうな予感です。

ただ、「長さを取り出す…？🤔」ってなりますよね。

そこは知ってるか知ってないかという話になってきて、さっき`T[0]`っていうのができましたよね。ということは`T[number]`自体が配列のように扱えるってことです。

普段配列の長さを取る時は`Array.length`としてると思うのですがこれと同じ要領で`T`の`length`プロパティにアクセスすればイイので、`T["length"]`とすれば長さを取り出せます。

あとは、`type Length<T>`に渡される方に注意する必要があります。今回は`Tuple`だけを受け取れるようにする必要があるので、`T`に制約をつけなければいけません。

こういうときは…？ (さんざんさっきまでの問題でも出てきてたし、分かるよね)

### 解答

```
type Length<T extends readonly any[]> = T['length']
```

## Exclude

```
type Result = MyExclude<'a' | 'b' | 'c', 'a'> // 'b' | 'c'
```

### 考え方

特定の型同士を比較して該当しないものは取り除く、ということなのでやっぱり`extends`を使いそう。

そういえば、`Distributive Conditional Types`とかいう特性を使うとUNION型を受け取って`extends`の結果をUNION型として返せたような…？

### 解答例

```
type MyExclude<T, U> = T extends U ? never : T
```

## Awaited

**個人的に初級編で2番目に難しいと思う問題がこれです。知ってるだけだとダメで、知識を応用して使う必要があります。**

```
type ExampleType = Promise<string>

type Result = MyAwaited<ExampleType> // string
```

### 考え方

`ExampleType`の`Promise<T>`の`T`を取り出せばいいので、これまでどおり`extends`で型を絞るにあたって「`T`ってこれじゃね？」っていう取り出し方ができる`infer`を使えばよさそう。

で、`extends`の条件に該当していれば推論した型を、そうでなければ違うものを返してあげればよさそう。

### この問題は難しいのでもうちょい解説

…と思うんだけどそうは問屋がおろしてくれない。その発想だけだと、

```
type MyAwaited<T> = T extends Promise<infer U> ? U : T
```

みたいな実装になるんですが、それだとこいつらを解決できない。

```
  Expect<Equal<MyAwaited<Z>, string | number>>,
  Expect<Equal<MyAwaited<Z1>, string | boolean>>,
  Expect<Equal<MyAwaited<T>, number>>,
```

まず第一に、

```
type T = { then: (onfulfilled: (arg: number) => any) => any }
```

とかいう独自実装された`then`を解決することを目指す。

これは知ってるか知ってないかという話で、`Promise`ではなくさっき出てきた`PromiseLike`を使えばうまく行きそう。

残るは

```
type Z = Promise<Promise<string | number>>
type Z1 = Promise<Promise<Promise<string | boolean>>>
```

これで、こいつは`Promise`のなかにある`Promise`を取り出してあげないといけない。

そのためには、`T extends Promise<infer U> ? U : T` で `U`がまた`Promise`ならもう一回判定して、最終的に`T`が帰ってくるまで判定し続ければいい。

というわけで、再帰させる。

### 解答例

```
type MyAwaited<T> = T extends PromiseLike<infer U> ? MyAwaited<U> : T
```

## If

```
type A = If<true, 'a', 'b'>; // expected to be 'a'
type B = If<false, 'a', 'b'>; // expected to be 'b'
```

### 考え方

なぜか突然簡単になって拍子抜けする。

型に対する`if`っていうのは`extends`なので、素直にやるだけ。ただし、`type error = If<null, 'a', 'b'>`というパターンがあるとおり、`C`が受け入れられるものは`Boolean`のみとする必要がある。

### 解答例

```
type If<C extends Boolean, T, F> = C extends true ? T : F;
```

## Concat

```
type Result = Concat<[1], [2]>; // expected to be [1, 2]
```

### 考え方

また簡単になって拍子抜けする。

`First of Array`や`Length of Tuple`でやったように、型定義の中でも配列は配列のように扱えます。

つまり、`...T`のようにできそう🤔

あとは、`as const`な`tuple`があるので受け入れる際の型に注意が必要そうです。

### 解答例

```
type Concat<T extends readonly unknown[], U extends readonly unknown[]> = [...T, ...U]
```

## Includes

```
type isPillarMen = Includes<['Kars', 'Esidisi', 'Wamuu', 'Santana'], 'Dio'> // expected to be `false`
```

### 考え方

個人的に初級編で1番難しい問題。~というか、どう考えても初級じゃない~

知ってる必要がある知識は`extends`と`infer`です。

おそらく、最初に想像するのはこういう感じのコードのはず。

```
type Includes<T extends readonly unknown[], U> = U extends T[number] ? true : false
```

けど、これだけだとこれらのパターンに対応できていません。

```
type cases = [
  Expect<Equal<Includes<[{}], { a: 'A' }>, false>>,
  Expect<Equal<Includes<[boolean, 2, 3, 5, 6, 7], false>, false>>,
  Expect<Equal<Includes<[true, 2, 3, 5, 6, 7], boolean>, false>>,
  Expect<Equal<Includes<[{ a: 'A' }], { readonly a: 'A' }>, false>>,
  Expect<Equal<Includes<[{ readonly a: 'A' }], { a: 'A' }>, false>>,
  Expect<Equal<Includes<[1], 1 | 2>, false>>,
  Expect<Equal<Includes<[1 | 2], 1>, false>>,
]
```

これらのパターンに共通しているのは、どうも`Includes<K, U>`で`K`と`U`が互いに互換性のある場合にうまくってないようです。

そこをクリアするためにはちょっとしたヒラメキが必要になります。

### もうちょい考える: 型の互換性チェック

この`K`, `U`の互換性をチェックする方法について確かめてみます。

```
type Test1 = [1, 1] extends [1, 1] ? true : false;  // true
type Test2 = [number, number] extends [number, number] ? true : false;  // true
type Test3 = [string, string] extends [string, string] ? true : false;  // true

type Test4 = [1, 2] extends [2, 1] ? true : false;  // false
type Test5 = [number, string] extends [string, number] ? true : false;  // false
type Test6 = [boolean, string] extends [string, boolean] ? true : false;  // false
```

どうやら`[K, U] extends [U, K]`と書けばイイようです。

### 解答例

```
type Includes<T extends readonly any[], U> =
T extends [infer K, ...infer Rest] ?
  [K, U] extends [U, K] ?
    Equal<U, K>
    : Includes<Rest, U>
  : false
```

まず`Includes<T extends readonly any[], U>`で、`Includes`の一つ目の型は読み取り専用で何かしらの配列を受け取るものとします。

次に`T extends [infer K, ...infer Rest]`で、配列の先頭要素と残りの要素で型を分けます。

これがなかなか面白いコードで、このように書くことで擬似的にループを表現することができるようになります。

どういうことかというと、

```
T extends [infer K, ...infer Rest] ?
  [K, U] extends [U, K] ?
    Equal<U, K>
    : Includes<Rest, U>
  : false
```

これにより、互換性がない場合は`false`となり、互換性があり`U, K`が同じ型であれば`true`が返ります。

残った`Includes<Rest, U>`ですが、こうして再び`Rest`を使って再起的に一つずつチェックをしていくというわけです。

~やっぱこれのどこが初級やねん~

## Push

```
type Result = Push<[1, 2], '3'> // [1, 2, '3']
```

### 考え方

難易度の上下がメンヘラすぎる。`Concat`を解いてればなんの問題もなく解けますよね。型を配列として素直に扱ってあげればよさそうです。

注意点としては、型を配列として扱うためにあらかじめ配列だけを受け入れるようにしておく必要がありそうです。

### 解答例

```
type Push<T extends any[], U> = [...T, U]
```

## Unshift

```
type Result = Unshift<[1, 2], 0> // [0, 1, 2]
```

### 考え方

`Push`と同じ。

### 解答例

```
type Unshift<T extends any[], U> = [U, ...T]
```

## Parameters

```
const foo = (arg1: string, arg2: number): void => {}

type FunctionParamsType = MyParameters<typeof foo> // [arg1: string, arg2: number]
```

### 考え方

「なんかこの問題例みたことあるぞ…？」ってなりますよね。そう、この記事の冒頭で出した`infer`のサンプルコードです。

あれは「`T`が関数型なら戻り値は`R`っぽいよ？」という`infer`の使用例です。今回は逆に「`T`が関数型なら引数は`R`っぽいよ？」ということをしてあげればよさそうです。

### 解答例

```
type MyParameters<T extends (...args: any[]) => any> = T extends  (...args: infer R) => any ? R : never
```

## おわりに

自分は初級編3周目くらいなのですが、「これどうやるっけ？」ってなるときがまだまだあります。[\[4\]](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fn-9d12-4)

もう少しやりこんでいって、頭と手にTypeScriptのルールを覚え込ませたいです。

いまのところ中級編も書いていこうと思ってます。が、ギブアップしたらゴメンナサイ…

脚注

1.  type-challengesをやりこむまで、私もめっちゃそうでした [↩︎](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fnref-9d12-1)
    
2.  実際に私自身がtype-challengesに挑戦しながら学習内容をまとめているだけとも言います。 [↩︎](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fnref-9d12-2)
    
3.  むしろ`infer`は`extends`の条件式でしか使えません [↩︎](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fnref-9d12-3)
    
4.  `Include`とか`Include`とか`Include`とか`Awaited`とか。 [↩︎](https://zenn.dev/yskn_sid25/articles/1b0b48d0a15426?s=09#fnref-9d12-4)