---
tags:
  - プログラミング言語
---

```go
// Arrayは固定長
var ary [2]string

/* 
SliceはArrayを特定の長さに切り取ったもの
一般的にSliceを使う
*/
var slice []int

/*
Array[start:end]でSliceになる
*/
array := [3]string{"hoge", "fuga", "piyo"}
slice := array[:]
fmt.Printf("%T\n", array) //=> [3]string
fmt.Printf("%T\n", slice) //=> []string

/*
SliceはArrayを参照しているに過ぎない
Sliceは容量(capacity, cap)と長さ(length, len)を持ち、capは参照元のArrayの要素数、lenはSliceの長さ
*/
s := []int{2,3,5,7,11,13}
s = s[:0] //=> cap=6, len=0, []
s = s[:4] //=> cap=6, len=4, [2,3,5,7]
s = s[2:] //=> cap=4, len=2, [5,7]
s = s[:4] //=> cap=4, len=4, [5,7,11,13]

/*
Sliceのゼロ値はnil
*/

s := []int{}
fmt.Println(s, len(s), cap(s)) //=>[] 0 0
fmt.Printf("%t\n", s == nil) //=> true
```

```go
/*
Map
*/
```
fmtパッケージ
標準出力に書式設定して出力する
```go
/* 
Printf()
第一引数に書式指定文字列を受け取り、第二引数以降に渡した値をフォーマットして出力
%d decimal(10進数)
%f float
%c char
%t bool
%s string
%p pointer
%v それぞれの型の既定の書式で表示
%T Type(変数の型)
*/
fmt.Printf("%d", 100) //=>100
fmt.Printf("%f", 3.1415) //=>3.141500
a := "hoge"
fmt.Printf("%c", a[0]) //=>h
fmt.Printf("%t", true) //=>100
fmt.Printf("%s", a) //=>hoge
fmt.Printf("%p", &a) //=>0xc00008e020
fmt.Printf("%v", a) //=>hoge
fmt.Printf("%T", a) //=>string

/*
Pringln()
値を標準の書式で出力する。Printf("%v")と同じ
値の間にwhite space, 終端に改行文字が入る
*/
a := "hoge"
b := "999"
fmt.Println(a, b) //=> hoge 999
```

```go
// 関数の定義
func MethodName(argument argumentType) returnType {
	...
}
```

```go
// Goにクラスはなく、structとポインタレシーバを持つメソッドで定義する
// メンバはlowerCamelCaseならprotected, UpperCamelCaseならpublic
type Person struct {
    name string
    age int
}

// クラスメソッドは(thisに相当する変数, *構造体名)をつける。*構造体名はポインタレシーバ
// setter
func (p *Person) SetPerson(name string, age int) {
    p.name = name
    p.age = age
}

// getter
func (p *Person) GetPerson() (string, int) {
    return p.name, p.age
}

func main() {
	// structの初期化はp1 := Person{"Yamada", 26} または Person{name: "Yamada", age: 26}とも書ける
    var p1 Person
    p1.SetPerson("Yamada", 26)
    name, age := p1.GetPerson()
    fmt.Printf("%s(%d)\n", name, age)
}
```

```go
// ポインタを引数に取る場合はポインタを渡さなければならない
type Arg struct {
	name string
}
func Something1(arg *Arg) {
	...
}

a1 := Arg{"a1"}
Something1(a1) // Compile error!
Something1(&a1) // OK

// ポインタレシーバの場合は変数でもポインタでもOK
func (arg *Arg)Something2 {
	...
}

a1.Something2 // OK メソッド内でポインタレシーバが自動的に呼び出される
(&a1).Something2 // OK

/*
	ポインタレシーバを使う理由は２つ
	- メソッドがレシーバが指す先の変数を変更するため
	- メソッドの呼び出しごとに変数のコピーを避けるため
*/
```

```go
// Interfaces

```