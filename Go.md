---
tags:
  - プログラミング言語
---
:
```go
// Arrayは固定長
var ary [2]string

// Sliceは可変長
var slice []int
```

fmtパッケージ
標準出力に書式設定して出力する
```go
/* 
Printf
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
a := "hoge"
fmt.Printf("%d", 100) //=>100
fmt.Printf("%f", 3.1415) //=>3.141500
fmt.Printf("%c", a[0]) //=>h
fmt.Printf("%t", true) //=>100
fmt.Printf("%s", a) //=>hoge
fmt.Printf("%p", &a) //=>100
fmt.Printf("%v", 100) //=>100
fmt.Printf("%T", 100) //=>100
```