---
tags:
  - npm
---
イミュータブルなstateを扱えるようにするライブラリ。

```js
const baseState = [
    {
        title: "Learn TypeScript",
        done: true
    },
    {
        title: "Try Immer",
        done: false
    }
]
// baseStateの2番目のオブジェクトをdone: trueにし、3番目に{title: "Tweet about it"}を追加する場合

// without immer
const nextState = baseState.slice() // shallow clone the array
nextState[1] = {
    // replace element 1...
    ...nextState[1], // with a shallow clone of element 1
    done: true // ...combined with the desired update
}
// since nextState was freshly cloned, using push is safe here,
// but doing the same thing at any arbitrary time in the future would
// violate the immutability principles and introduce a bug!
nextState.push({title: "Tweet about it"})

// with immer
import {produce} from "immer"

// produce関数は第一引数に元となるstate、第二引数にstateの変更処理を定義したコールバック関数を受け取る。
const nextState = produce(baseState, draft => {
    draft[1].done = true
    draft.push({title: "Tweet about it"})
})
```


[immerjs/immer: Create the next immutable state by mutating the current one](https://github.com/immerjs/immer)
[Introduction to Immer | Immer](https://immerjs.github.io/immer/)